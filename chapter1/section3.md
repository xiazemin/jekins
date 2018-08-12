# Jenkins Git安装设置

在这个练习中，必须确保Internet连接可连接其安装 Jenkins 机器。在 Jenkins 仪表盘（主屏幕）的左侧单击 Manage Jenkins 选项。打开网址：[http://localhost:8080/jenkins](http://localhost:8080/jenkins)

在接下来的屏幕上，单击“Manage Plugins”选项。

在接下来的屏幕中，单击 Available 选项卡。这个标签将显示可供下载的插件列表。在 “Filter” 选项卡类型“Git plugin”。

该列表将被过滤。找到 “GIT plugin”选项，并单击按钮 ‘Install without restart’，开始下载安装。

安装将开始，屏幕将被刷新，显示下载的状态。

Installing Plugins Upgrades

一旦所有的安装完成后，通过在浏览器中执行以下命令（URL地址）重新启动 Jenkins。URL地址：[http://localhost:8080/jenkins/restart](http://localhost:8080/jenkins/restart)

Jenkins 重新启动后，Git会可作为配置的同时也可以作业的选项。 要验证，单击新建项目在 Jenkins 的菜单选项。然后输入一个名称的工作，在以下示例中，输入的名称是“Demo”。选择“Freestyle project”作为项目类型。单击确定按钮。

New Item Jenkins

在接下来的屏幕上，如果你浏览到的源代码管理（Source code Management ）部分，会看到现在 'Git' 作为一个选项。

![](/assets/import1.png)

