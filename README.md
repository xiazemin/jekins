# Introduction

安装jenkins有两种方式，tomcat方式部署和java部署启动。本次实验我以tomcat下部署启动为例。



（1）tomcat方式部署



a.首先安装tomcat和JAVA，配置环境变量（此步骤不再讲述，java配置不可缺少）



我这里安装的是jdk 1.8.0\_65。



 



b.将从官网下载下来的jenkins.war文件放入tomcat下的webapps目录下，进入tomcat的/bin目录下，启动tomcat即启动jenkins。



我这里用的是tomcat8。



 



c.启动jenkins时，会自动在webapps目录下建立jenkins目录，访问地址为：http://localhost:8080/jenkins



1

2

3

4

5

\[jenkins@osb30 ~\]$ tar zxf apache-tomcat-8.0.30.tar.gz

\[jenkins@osb30 ~\]$ mv jenkins.war apache-tomcat-8.0.30/webapps/

\[jenkins@osb30 ~\]$ cd apache-tomcat-8.0.30

\[jenkins@osb30 apache-tomcat-8.0.30\]$ bin/startup.sh

Jenkins home directory: /home/jenkins/.jenkins found at: $user.home/.jenkins

如果启动时报错：



1

Caused by:java.awt.AWTError: Can't connect to X11 window server using ':0' as the value of the DISPLAY varible...

解决：



1

2

3

\[jenkins@osb30 ~\]$ cd apache-tomcat-8.0.30/bin/

\[jenkins@osb30 bin\]$ vim catalina.sh

JAVA\_OPTS="-Xms1024m -Xmx1024m -Djava.awt.headless=true"

 



d.访问jenkins



http://172.16.206.30:8080/jenkins



 



（2）java部署启动jenkins



切换到jenkins.war存放的目录，输入如下命令：



1

$ java -jar jenkins.war

可以修改启动端口



1

$ java -jar jenkins.war --httpPort=8000

然后在浏览器中（推荐用火狐、chrom）输入http://localhost:8080，localhost可以是本机的ip，也可以是计算机名。就可以打开jenkins；修改端口后，访问地址的端口需同步变更。

