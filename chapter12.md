# Jenkins + Docker

![](/assets/importjd.png)大体结构

开发人员在gitLab上打了一个tag

gitLab把tag事件推送到Jenkins

Jenkins 获取tag源码，编译，打包，构建镜像

Jenkins push 镜像到阿里云仓库

Jenkins 执行远程脚本

5-1. 远程服务器 pull 指定镜像

5-2. 停止老版本容器，启动新版本容器

通知测试人员部署结果

基于maven构建镜像，上传阿里云docker仓库

maven 构建镜像配置

pom.xml 文件添加docker插件

&lt;plugin&gt;

```
&lt;groupId&gt;com.spotify&lt;/groupId&gt;

&lt;artifactId&gt;docker-maven-plugin&lt;/artifactId&gt;

&lt;version&gt;0.4.11&lt;/version&gt;

&lt;configuration&gt;

    &lt;imageName&gt;${docker.image.prefix}/${project.artifactId}&lt;/imageName&gt;

    &lt;imageTags&gt;

        &lt;imageTag&gt;${project.version}&lt;/imageTag&gt;

        &lt;imageTag&gt;latest&lt;/imageTag&gt;

    &lt;/imageTags&gt;

    &lt;dockerDirectory&gt;src/main/docker&lt;/dockerDirectory&gt;

    &lt;resources&gt;

        &lt;resource&gt;

            &lt;targetPath&gt;/&lt;/targetPath&gt;

            &lt;directory&gt;${project.build.directory}&lt;/directory&gt;

            &lt;include&gt;${project.build.finalName}.jar&lt;/include&gt;

        &lt;/resource&gt;

    &lt;/resources&gt;

&lt;/configuration&gt;
```

&lt;/plugin&gt;

${docker.image.prefix} 是镜像的前缀

${project.artifactId} 是镜像名字

${project.version} 版本号，此处也用来当做镜像的版本号

docker-maven-plugin 使用可以自行百度。

src/main/docker 目录下新增文件 Dockerfile，内容如下：

FROM frolvlad/alpine-oraclejdk8:slim

VOLUME /tmp

ADD demo-service-ver-0.0.1.jar app.jar

RUN sh -c 'touch /app.jar'

ENV JAVA\_OPTS=""

ENTRYPOINT \[ "sh", "-c", "java $JAVA\_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" \]

以上的配置可以把一个服务打包成镜像，只需要执行

$ sudo mvn package docker:build

阿里云docker仓库使用

注册或者启用阿里云docker仓库就不赘述。下面简单介绍上传拉取docker镜像

登录

$ sudo docker login --username=用户名 --password=密码 registry-internal.cn-hangzhou.aliyuncs.com

push 之前生成的镜像

$ sudo docker tag \[ImageId\] registry.cn-hangzhou.aliyuncs.com/xxx/demo-service:\[镜像版本号\]

$ sudo docker push registry.cn-hangzhou.aliyuncs.com/xxx/demo-service:\[镜像版本号\]

xxx : 是你镜像仓库的namespace

一堆push后，你就可以在阿里云的Docker镜像仓库里面看到你对应的镜像了。下图是我们公司的部分镜像列表

镜像列表

镜像列表

pull 镜像

登录操作同上

$ sudo docker pull  registry.cn-hangzhou.aliyuncs.com/xxx/demo-service:\[镜像版本号\]

jenkins 部署配置

构建Jenkins镜像

FROM jenkins

USER root

RUN apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/\*

RUN echo "jenkins ALL=NOPASSWD: ALL" &gt;&gt; /etc/sudoers

USER jenkins

一开始使用官方的镜像直接启用，一些插件和配置多少都有点问题，比如不能使用sudo，等等。因此基于官方镜像构建一个更符合我们需要的镜像。

构建命令：

$ sudo docker build -t buxiaoxia/jenkins:1.0

启动Jenkins容器

sudo docker run --memory 1.5G  --name buxiaoxia-jenkins \

-p 18181:8080 -p 50000:50000 -u root -d \

--env JAVA\_OPTS="-Xms256m -Xmx512m  -XX:MaxNewSize=256m"  \

-v /var/run/docker.sock:/var/run/docker.sock   \

-v /usr/bin/docker:/usr/bin/docker  \

-v /home/buxiaoxia/software/jenkins:/var/jenkins\_home  \

-v /usr/lib64/libltdl.so.7:/usr/lib/x86\_64-linux-gnu/libltdl.so.7 \

buxiaoxia/jenkins:1.0

简单解释下：

-v /var/run/docker.sock:/var/run/docker.sock 与 -v /usr/bin/docker:/usr/bin/docker是把宿主机docker 映射到容器内。

-v /home/buxiaoxia/software/jenkins:/var/jenkins\_home 指定Jenkins的宿主机存储路径

-v /usr/lib64/libltdl.so.7:/usr/lib/x86\_64-linux-gnu/libltdl.so.7 在centos7 系统下会出现个别的包丢失，对应的引下宿主机的包就可以。

docker在容器内构建的时候，如果出现权限不够什么的。可以在宿主机中使用以下两种方式：

$ sudo chmod 777 /var/run/docker.sock

或者

$ usermod -a -G docker jenkin

jenkins 启动后，访问对应的Jenkins页面，初始化只要一步步跟着走就可以了。

Jenkins配置

插件下载

所需要的插件：

Maven Integration plugin

docker-build-step

Docker plugin

Gitlab Hook Plugin

GitLab Plugin

因为使用的是gitlab，对应的插件也是必须的。下载完插件后，maven等相关插件配置好（此处省略...）

maven 镜像地址配置

可以直接在宿主机修改，路径在:/home/buxiaoxia/software/jenkins/tools/hudson.tasks.Maven\_MavenInstallation/maven3-1/conf 下的settings.xml

setting.xml 镜像改成阿里云的就OK，飞起。。。

新建一个maven job

源码配置

![](/assets/importgitlab.png)

构建

![](/assets/importbuild.png)

构建后执行特定脚本

![](/assets/importstep.png)

脚本内容如下：

echo '================开始推送镜像================'

sudo docker login --username=用户名 --password=密码 registry-internal.cn-hangzhou.aliyuncs.com

sudo docker push registry-internal.cn-hangzhou.aliyuncs.com/xxx/demo-service

echo '================结束推送镜像================'

echo '================开始远程启动================'

ssh buxiaoxia@192.168.100.2 -tt &lt;&lt; remotessh            \#\#\#首先要ssh上去注意这里的&lt;&lt; remotessh，需要做公钥密钥

\#\#\#\#从这里开始都是在远程机器上执行命令

cd /home/buxiaoxia/xiaw

./jenkins.sh registry-internal.cn-hangzhou.aliyuncs.com/xxx/demo-service

sudo docker login --username=用户名 --password=密码 registry-internal.cn-hangzhou.aliyuncs.com

sudo docker pull registry-internal.cn-hangzhou.aliyuncs.com/xxx/demo-service

sudo docker run -d -m 300m  --name=demo-service-\`date +%Y-%m-%d\` --restart=always registry-internal.cn-hangzhou.aliyuncs.com/xxx/demo-service

echo "finished!"

\#\#\#\#\#执行完毕

exit  \#\#\#退出远程机器

remotessh  \#\#\#结尾哦

echo '================结束远程启动================'

jenkins.sh 脚本内容：

\#!/bin/sh

sudo docker stop $\(sudo docker ps \| grep $1\|awk '{print  $1}'\|sed 's/%//g'\)

以上就完成了一个简单的自动化构建

gitlab配置webhook

Jenkins安装完对应的gitlab插件，配置中的构建触发选择如下

![](/assets/importwebhook.png)

复制红框中的url

![](/assets/importweburl.png)

再在gitlab的对应项目中webhooks页面中的url填入前面复制的url

保存即可，右下角可以点击测试哦。

配置完成后，每次该项目有个tag push event ，都会触发Jenkins的自动构建。接着，Jenkins就执行 拉取源码 -&gt; 编译 -&gt; 构建镜像 -&gt; 推送镜像 -&gt; 执行远程启动脚本完成部署。

总结

一步步的配置，基本就跑通了我们基于Jenkins，docker实现自动化部署的初始版本。开发人员完成功能开发后，需要交互一个测试版本，只需要推送一个tag到git仓库，就能够将代码自动部署到特定的服务器上。可喜可贺~ 可以省去一堆的从一个服务器跑到另一个服务器，然后执行各种命令的琐碎操作。。。

关于配置

目前我是使用了consul的配置共享，把不同环境的配置放在了consul上，镜像中没有保留可变的配置，而是根据启动的参数就可以自由切换环境配置。

当然，consul的配置共享可以看看我git上关于consul的项目：[http://git.oschina.net/buxiaoxia/spring-demo](http://git.oschina.net/buxiaoxia/spring-demo)

存在问题

docker未使用编排，较为独立，需要知道特定的服务器网络位置

docker镜像的push与pull，都需要明文执行阿里云账号密码，可进一步改进

未构建版本回退流程

shell脚本健壮性不够，异常未处理

优化

可以针对以上问题做相应的优化，完善初始化版本的CD流程。例如，docker 使用swarm，让swarm管理docker 容器等等

