# 下载Jenkins

对于Jenkins官方网站是Jenkins。如果点击给出的链接，就可以访问 [Jenkins官方网站](https://jenkins-ci.org/)的首页了

linuxprobe官方网站

下载Jenkins

对于Jenkins官方网站是Jenkins。如果点击给出的链接，就可以访问 Jenkins官方网站的首页了，如下图所示。

默认情况下，最新版本和长期支持版本可供下载。以前版本也可供下载。点击下载区长期支持版本选项卡。

点击链接下载Jenkins.war 文件，这里选择的是最新版本。如下图所示：

启动Jenkins

打开命令提示符。在命令提示符下，浏览到 jenkins.war 文件存在的目录。运行以下命令：

D:\worksp\yiibai.com&gt;java -jar Jenkins.war --httpPort=8080

该命令后，各项任务将运行，其中一个是由名为 winstone 的嵌入式web服务器进行 war 文件提取。

D:\worksp\yiibai.com&gt;java -jar Jenkins.war

Running from: D:\worksp\yiibai.com\jenkins.war

webroot: $user.home/.jenkins

十二月 19, 2015 12:32:19 上午 winstone.Logger logInternal

信息: Beginning extraction from war file

十二月 19, 2015 12:32:19 上午 org.eclipse.jetty.util.log.JavaUtilLog info

信息: jetty-winstone-2.9

十二月 19, 2015 12:32:24 上午 org.eclipse.jetty.util.log.JavaUtilLog info

信息: NO JSP Support for , did not find org.apache.jasper.servlet.JspServlet

Jenkins home directory: C:\Users\Administrator.jenkins found at: $user.home/.j

nkins

十二月 19, 2015 12:32:24 上午 org.eclipse.jetty.util.log.JavaUtilLog info

信息: Started SelectChannelConnector@0.0.0.0:8080

十二月 19, 2015 12:32:24 上午 winstone.Logger logInternal

信息: Winstone Servlet Engine v2.0 running: controlPort=disabled

十二月 19, 2015 12:32:25 上午 jenkins.InitReactorRunner$1 onAttained

信息: Started initialization

一旦处理是完全没有严重错误，在命令提示符会输出以下行。

INFO: Jenkins is fully up and running

访问Jenkins

一旦 Jenkins 已经启动并运行，可以从以下链接访问 Jenkins −[http://localhost:8080](http://localhost:8080)

打开此链接后将出现Jenkins 仪表板。

