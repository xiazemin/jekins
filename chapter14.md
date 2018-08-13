# spring boot docker

Docker服务中进程间通信通过/var/run/docker.sock实现，默认服务不提供监听端口，因此使用docker remote api 需要手动绑定端口。



 



在centos7.2下，可以进行这样的操作：

直接在ExecStart=/usr/bin/dockerd后面增加需要的启动参数即可。我这里增加了DockerHub镜像加速地址和开启了tcp连接（--registry-mirror=https://xxoo.mirror.acs.aliyun.com -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock） 

vim /etc/systemd/system/docker.service

ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock





systemctl daemon-reload

systemctl restart docker



 



实际操作：



\[root@localhost frinder\]\# vi /usr/lib/systemd/system/docker.service

在：ExecStart=/usr/bin/dockerd-current 后面追加：-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock



\[root@localhost frinder\]\# cat /usr/lib/systemd/system/docker.service

\[Unit\]

Description=Docker Application Container Engine

Documentation=http://docs.docker.com

After=network.target

Wants=docker-storage-setup.service

Requires=docker-cleanup.timer



\[Service\]

Type=notify

NotifyAccess=all

EnvironmentFile=-/etc/sysconfig/docker

EnvironmentFile=-/etc/sysconfig/docker-storage

EnvironmentFile=-/etc/sysconfig/docker-network

Environment=GOTRACEBACK=crash

Environment=DOCKER\_HTTP\_HOST\_COMPAT=1

Environment=PATH=/usr/libexec/docker:/usr/bin:/usr/sbin

ExecStart=/usr/bin/dockerd-current -H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock \

--add-runtime docker-runc=/usr/libexec/docker/docker-runc-current \

--default-runtime=docker-runc \

--exec-opt native.cgroupdriver=systemd \

--userland-proxy-path=/usr/libexec/docker/docker-proxy-current \

$OPTIONS \

$DOCKER\_STORAGE\_OPTIONS \

$DOCKER\_NETWORK\_OPTIONS \

$ADD\_REGISTRY \

$BLOCK\_REGISTRY \

$INSECURE\_REGISTRY

ExecReload=/bin/kill -s HUP $MAINPID

LimitNOFILE=1048576

LimitNPROC=1048576

LimitCORE=infinity

TimeoutStartSec=0

Restart=on-abnormal

MountFlags=slave



\[Install\]

WantedBy=multi-user.target

\[root@localhost frinder\]\# systemctl daemon-reload 

\[root@localhost frinder\]\# systemctl restart docker.service





客户端（idea）连接：



docker服务端设置好端口后，客户端配置 API URL为：服务器host+port，如，服务端ip：192.168.98.128，端口：2376，则配置为：http://192.168.98.128:2376



 



 



 



 



项目配置：





pom.xml中添加支持：

&lt;docker.image.prefix&gt;spring.io&lt;/docker.image.prefix&gt;

&lt;plugin&gt;

&lt;groupId&gt;com.spotify&lt;/groupId&gt;

&lt;artifactId&gt;docker-maven-plugin&lt;/artifactId&gt;

&lt;version&gt;0.4.11&lt;/version&gt;

&lt;configuration&gt;

&lt;dockerHost&gt;http://192.168.98.128:2376&lt;/dockerHost&gt;

&lt;imageName&gt;${docker.image.prefix}/${project.artifactId}&lt;/imageName&gt;

&lt;dockerDirectory&gt;src/main/docker&lt;/dockerDirectory&gt;

&lt;resources&gt;

&lt;resource&gt;

&lt;targetPath&gt;/&lt;/targetPath&gt;

&lt;directory&gt;${project.build.directory}&lt;/directory&gt;

&lt;include&gt;${project.build.finalName}.jar&lt;/include&gt;

&lt;/resource&gt;

&lt;/resources&gt;

&lt;/configuration&gt;

&lt;/plugin&gt;



在项目 main目录下添加目录 docker，添加文件 Dockerfile，内容如下：

FROM frolvlad/alpine-oraclejdk8:slim

VOLUME /tmp

ADD docker.app-1.0-SNAPSHOT.jar app.jar

RUN sh -c 'touch /app.jar'

ENV JAVA\_OPTS=""

ENTRYPOINT \[ "sh", "-c", "java $JAVA\_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" \]





设置 Artifacts：







 





注意点：

Output directory 设置，设置为 docker，即 Dockerfile同目录

jar包名称一定要与 Dockerfile中的一致，不然会报找不到文件的错误





推送到 docker：

clean package docker:build -DpushImage



 



push过程中会报错，忽略之，docker中是可以启动成功的！错误信息如下：



......

\[WARNING\] Failed to push spring.io/docker.app, retrying in 10 seconds \(5/5\).

\[INFO\] Pushing spring.io/docker.app

The push refers to a repository \[spring.io/docker.app\]

b5e9a3e6075a: Preparing 

08ba4e16adba: Preparing 

162f935b1198: Preparing 

dcf909146faa: Preparing 

23b9c7b43573: Preparing 

\[INFO\] ------------------------------------------------------------------------

\[INFO\] BUILD FAILURE

\[INFO\] ------------------------------------------------------------------------

\[INFO\] Total time: 01:46 min

\[INFO\] Finished at: 2017-04-11T16:37:15+08:00

\[INFO\] Final Memory: 31M/75M

\[INFO\] ------------------------------------------------------------------------

\[ERROR\] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build \(default-cli\) on project docker.app: Exception caught: Error: Status 405 trying to push repository docker.app: "" -&gt; \[Help 1\]

\[ERROR\] 

\[ERROR\] To see the full stack trace of the errors, re-run Maven with the -e switch.

\[ERROR\] Re-run Maven using the -X switch to enable full debug logging.

\[ERROR\] 

\[ERROR\] For more information about the errors and possible solutions, please read the following articles:

\[ERROR\] \[Help 1\] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException



Process finished with exit code 1



 





检查镜像，发现 spring.io/docker.app 已经创建，启动 spring.boot项目 docker run -p 8080:8080 -t spring.io/docker.app 可以正常启动！



\[root@localhost frinder\]\# docker images

REPOSITORY TAG IMAGE ID CREATED SIZE

spring.io/docker.app latest 93ab985f7835 18 minutes ago 195.3 MB

docker.io/centos latest a8493f5f50ff 4 days ago 192.5 MB

docker.io/frolvlad/alpine-oraclejdk8 slim 00d8610f052e 5 weeks ago 166.6 MB

\[root@localhost frinder\]\# docker run -p 8080:8080 -t spring.io/docker.app



. \_\_\_\_ \_ \_\_ \_ \_

/\\ / \_\_\_'\_ \_\_ \_ \_\(\_\)\_ \_\_ \_\_ \_ \ \ \ \

\( \( \)\\_\_\_ \| '\_ \| '\_\| \| '\_ \/ \_\` \| \ \ \ \

\\/ \_\_\_\)\| \|\_\)\| \| \| \| \| \|\| \(\_\| \| \) \) \) \)

' \|\_\_\_\_\| .\_\_\|\_\| \|\_\|\_\| \|\_\\_\_, \| / / / /

=========\|\_\|==============\|\_\_\_/=/\_/\_/\_/

:: Spring Boot :: \(v1.5.2.RELEASE\)



2017-04-11 08:43:59.100 INFO 6 --- \[ main\] com.s.b.d.App : Starting App v1.0-SNAPSHOT on 07a7a077770d with PID 6 \(/app.jar started by root in /\)

2017-04-11 08:43:59.109 INFO 6 --- \[ main\] com.s.b.d.App : No active profile set, falling back to default profiles: default

2017-04-11 08:43:59.283 INFO 6 --- \[ main\] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@4b9af9a9: startup date \[Tue Apr 11 08:43:59 GMT 2017\]; root of context hierarchy

2017-04-11 08:44:01.873 INFO 6 --- \[ main\] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port\(s\): 8080 \(http\)

2017-04-11 08:44:01.905 INFO 6 --- \[ main\] o.apache.catalina.core.StandardService : Starting service Tomcat

2017-04-11 08:44:01.908 INFO 6 --- \[ main\] org.apache.catalina.core.StandardEngine : Starting Servlet Engine: Apache Tomcat/8.5.11

2017-04-11 08:44:02.097 INFO 6 --- \[ost-startStop-1\] o.a.c.c.C.\[Tomcat\].\[localhost\].\[/\] : Initializing Spring embedded WebApplicationContext

2017-04-11 08:44:02.098 INFO 6 --- \[ost-startStop-1\] o.s.web.context.ContextLoader : Root WebApplicationContext: initialization completed in 2826 ms

2017-04-11 08:44:02.322 INFO 6 --- \[ost-startStop-1\] o.s.b.w.servlet.ServletRegistrationBean : Mapping servlet: 'dispatcherServlet' to \[/\]

2017-04-11 08:44:02.334 INFO 6 --- \[ost-startStop-1\] o.s.b.w.servlet.FilterRegistrationBean : Mapping filter: 'characterEncodingFilter' to: \[/\*\]

2017-04-11 08:44:02.336 INFO 6 --- \[ost-startStop-1\] o.s.b.w.servlet.FilterRegistrationBean : Mapping filter: 'hiddenHttpMethodFilter' to: \[/\*\]

2017-04-11 08:44:02.336 INFO 6 --- \[ost-startStop-1\] o.s.b.w.servlet.FilterRegistrationBean : Mapping filter: 'httpPutFormContentFilter' to: \[/\*\]

2017-04-11 08:44:02.336 INFO 6 --- \[ost-startStop-1\] o.s.b.w.servlet.FilterRegistrationBean : Mapping filter: 'requestContextFilter' to: \[/\*\]

2017-04-11 08:44:02.822 INFO 6 --- \[ main\] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@4b9af9a9: startup date \[Tue Apr 11 08:43:59 GMT 2017\]; root of context hierarchy

2017-04-11 08:44:03.006 INFO 6 --- \[ main\] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{\[/error\],produces=\[text/html\]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml\(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse\)

2017-04-11 08:44:03.008 INFO 6 --- \[ main\] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{\[/error\]}" onto public org.springframework.http.ResponseEntity&lt;java.util.Map&lt;java.lang.String, java.lang.Object&gt;&gt; org.springframework.boot.autoconfigure.web.BasicErrorController.error\(javax.servlet.http.HttpServletRequest\)

2017-04-11 08:44:03.066 INFO 6 --- \[ main\] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path \[/webjars/\*\*\] onto handler of type \[class org.springframework.web.servlet.resource.ResourceHttpRequestHandler\]

2017-04-11 08:44:03.066 INFO 6 --- \[ main\] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path \[/\*\*\] onto handler of type \[class org.springframework.web.servlet.resource.ResourceHttpRequestHandler\]

2017-04-11 08:44:03.150 INFO 6 --- \[ main\] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path \[/\*\*/favicon.ico\] onto handler of type \[class org.springframework.web.servlet.resource.ResourceHttpRequestHandler\]

2017-04-11 08:44:03.191 INFO 6 --- \[ main\] oConfiguration$WelcomePageHandlerMapping : Adding welcome page: class path resource \[templates/index.html\]

2017-04-11 08:44:03.411 INFO 6 --- \[ main\] o.s.j.e.a.AnnotationMBeanExporter : Registering beans for JMX exposure on startup

2017-04-11 08:44:03.506 INFO 6 --- \[ main\] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port\(s\): 8080 \(http\)

2017-04-11 08:44:03.518 INFO 6 --- \[ main\] com.s.b.d.App : Started App in 5.108 seconds \(JVM running for 5.93\)





已经可以访问了！

