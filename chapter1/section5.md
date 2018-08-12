# Jenkins配置

可以通过点击左侧菜单侧的 “Manage Jenkins”选项获得Jenkins 的各种配置选项。



然后，您将看到下面的画面 -

![](/assets/importC.png)

单击 “Configure system”。下面讨论是一些可被执行 Jenkins 配置设置。

Jenkins主目录

Jenkins需要一定的磁盘空间来进行构建和保持档案。人们可以从 Jenkins 配置屏幕查看该路径。默认情况下，它被设置到 ~/.jenkins，而这个位置将首先在用户配置文件的位置存储起来。在正确的环境中，需要更改此位置，适当的位置来存储所有相关的建立和档案。可以通过以下方式做到这一点



启动 Servlet 容器之前，设置“JENKINS\_HOME”环境变量设置为新的主目录。

设置 “JENKINS\_HOME” 系统属性 到 servlet 容器。

设置JNDI环境条目“JENKINS\_HOME”到新目录。

下面的例子将使用“JENKINS\_HOME”环境变量设置的第一个选项。

首先创建一个新的文件夹：D:\worksp\yiibai.com\tomcat7\webapps\jenkins。复制所有内容从现有的 〜/.jenkins 到这个新的目录。

设置 JENKINS\_HOME 环境变量指向到安装在机器上 Java 的基本目录位置。 例如，

OS	输出

Windows	设置环境变量 JENKINS\_HOME 到你想要的位置。举个例子，可以将其设置为E：\ APPS \詹金斯 E:\Apps\Jenkins

Linux	export JENKINS\_HOME =/usr/local/Jenkins 或所希望的位置。

在 Jenkins 的仪表板，请在左侧菜单中选择管理 Jenkins。然后从右侧单击“Configure System”。

在主目录中，将看到已经配置了新的目录。

Jenkins Home Directory

\# of executors



 

这是指并发的作业的执行，可以发生在 Jenkins 机上的总数。这可以根据要求改变。有时建议是保持这个数目相同CPU数量的机器上实现性能更好。

Environment variables

这被用于添加将适用于所有作业定制的环境变量。这些是键 - 值对，并可以访问和用于在任何需要的地方构建。

Jenkins URL

默认情况下，Jenkins URL指向本地主机：localhost。 如果为您的机器设置一个域名，将其设置为域名别的覆盖本地主机与计算机的IP地址。这将有助于建立从站和在使用电子邮件作为使用环境变量 JENKINS\_URL 可以用于直接访问Jenkins URL发送链接为：${JENKINS\_URL}。



Email Notification

在电子邮件通知区域，可以向用户发送电子邮件配置SMTP设置。Jenkins连接到SMTP邮件服务器发送电子邮件到收件人列表，这是必需的。

