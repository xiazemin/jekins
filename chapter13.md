# 微服务之一键部署git+maven+jenkins+docker

默认已经装好Jenkins、maven、docker和git，我的Jenkins也在docker容器中运行的，由于在Jenkins容器中默认无法使用docker命令，所以在Jenkins启动时需要加入以下参数：



-v /var/run/docker.sock:/var/run/docker.sock



-v $\(which docker\):/usr/bin/docker



如果还是无法使用，为了省时，推荐使用下面集成好的Jenkins镜像，Dockerfile：



FROM jenkins



 



USER root



\#清除了基础镜像设置的源，切换成阿里云的jessie源



RUN echo'' &gt; /etc/apt/sources.list.d/jessie-backports.list \



  && echo"deb http://mirrors.aliyun.com/debian jessie main contrib non-free" &gt; /etc/apt/sources.list \



  && echo"deb http://mirrors.aliyun.com/debian jessie-updates main contrib non-free" &gt;&gt; /etc/apt/sources.list \



  && echo"deb http://mirrors.aliyun.com/debian-security jessie/updates main contrib non-free" &gt;&gt; /etc/apt/sources.list



\#更新源并安装缺少的包



RUN apt-get update && apt-get install -y libltdl7



 



ARG dockerGid=999



 



RUN echo"docker:x:${dockerGid}:jenkins" &gt;&gt; /etc/group \



USER jenkins



 



同时启动时还是得加上面提到的参数，启动命令如下：



$ docker run --name jenkins -p 7080:8080 -p 50000:50000 -v /etc/timezone:/etc/timezone -v /etc/localtime:/etc/localtime -v /var/docker\_data/jenkins/jenkins\_home:/var/jenkins\_home -v /var/docker\_data/jenkins/settings:/var/settings -v /var/run/docker.sock:/var/run/docker.sock -v $\(which docker\):/usr/bin/docker -d my-jenkins

 



正式开始：



1、 在程序的Pom.xml文件中加入docker-maven-plugin插件，如下：



&lt;plugin&gt;

   &lt;groupId&gt;com.spotify&lt;/groupId&gt;

   &lt;artifactId&gt;docker-maven-plugin&lt;/artifactId&gt;

   &lt;version&gt;1.0.0&lt;/version&gt;

   &lt;configuration&gt;

      &lt;dockerHost&gt;http://192.168.100.100:2375&lt;/dockerHost&gt;

      &lt;imageName&gt;${project.artifactId}&lt;/imageName&gt;

      &lt;dockerDirectory&gt;src/main/docker&lt;/dockerDirectory&gt;

      &lt;resources&gt;

         &lt;resource&gt;

            &lt;targetPath&gt;/&lt;/targetPath&gt;

            &lt;directory&gt;${project.build.directory}&lt;/directory&gt;

            &lt;include&gt;\*.jar&lt;/include&gt;

         &lt;/resource&gt;

      &lt;/resources&gt;

   &lt;/configuration&gt;

&lt;/plugin&gt;

需要说明的是dockerHost指定的是使用哪个主机的docker，如果不填写，则默认为本机，由于我的Jenkins在docker容器中与宿主机互不相通，所以我指定了宿主机的ip和端口。



dockerDirectory指定了你的Dockerfile所在的位置。







2、 在项目的根目录下新建一个shell脚本，我的是build.sh，脚本内容就是重新启动docker。







3、 Jenkins中配置maven：点击“系统管理”--&gt;“GlobalTool Configuration”--&gt;“maven安装”，选择install from Apavhe版本为3.5.0，勾选自动安装。



4、 然后新建一个自由风格的项目，完成后点击“配置”，源码管理选择git并填写项目的git地址。构建触发器选择Poll SCM，填写“\*/1 \* \* \* \*”，意思是一分钟去查询一次git源码如果有更新则会自动构建。其他随意。



5、 构建环节：选择Invoke top-level Maventargets，选择第三步中配置的maven3.5.0，Goals填写package -e -X docker:build -DskipTest -DdockerImageTags=latest，意思是使用maven的插件构建镜像，具体参数可以百度。



6、 构建环节：接着第五步，再增加一个构建步骤放下面，选择Executeshell，Command填写：



\#!/bin/bash



sh build.sh



就是执行项目根目录的那个脚本文件。如下图：







7、 最后一点，也是很重要的一点，就是必须给docker配置一个服务监听端口，找到docker.service文件，在“ExecStart参数”后面加上“ -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock”，命令如下：



vim /usr/lib/systemd/system/docker.service



ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock



 



 



systemctl daemon-reload



systemctl restart docker



              



第三章 常见错误



1、Caused by:com.spotify.docker.client.exceptions.DockerException: java.util.concurrent.ExecutionException:com.spotify.docker.client.shaded.javax.ws.rs.ProcessingException:java.io.IOException: Permission denied



解决：权限问题，表明Jenkins容器中无法使用docker命令。参考第二章的“前置环境”解决。



2、\[ERROR\]Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build\(default-cli\) on project springboot: Exception caught: Request error: POSTunix://localhost:80/build?t=springboot: 400: HTTP 400 Bad Request



解决：两个问题，一是docker的监听端口可能没配置或配置不成功；而是docker-maven-plugin版本问题，建议升级至1.0.0版本。

