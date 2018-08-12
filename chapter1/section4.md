# Jenkins Maven安装设置

第1步：下载和设置Maven

Maven的官方网站就是Apache Maven。如果点击给出的链接，就可以打开 Maven 的官方网站的首页，如下图所示。



浏览该网站，进入文件部分并下载链接二进制的 .zip 文件。



一旦文件被下载，解压缩文件到相关的应用程序文件夹。为了这个目的，Maven 的文件将被放置在 D:\worksp\yiibai.com\apache-maven-3.3.9



第2步：设置 Jenkins 和 Maven

在 Jenkins 仪表盘\(主屏幕\)，请在左侧菜单中选择管理 Jenkins。打开网址：http://localhost:8080/jenkins



然后，从右侧单击“Manage Jenkins”。



再点击“Configure Sytem"，结果如下图所示：

![](/assets/import3.png)

在配置系统屏幕上滚动，直至看到 Maven 部分，然后点击“Add Maven'按钮。

取消选中“Install automatically”选项。

添加名称和设置 MAVEN\_HOME 的位置。

然后，在屏幕的末尾点击“Save”按钮。



现在，可以创建一个‘Maven project’选项的作业。在Jenkins仪表盘，单击新建项目选项。



完成后如下图所示：

![](/assets/importM.png)

