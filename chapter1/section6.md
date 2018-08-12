# Jenkins管理

要管理Jenkins，点击从左边的菜单侧“Manage Jenkins”选项。

因此，可以通过点击左侧菜单侧 “Manage Jenkins”选项获得 Jenkins 各种配置选项。



一些管理选项如下 -

配置系统

这是其中一个可以管理路径的各种工具的使用建立，如：JDK，Ant和Maven 版本，以及安全选项，邮件服务器和其他系统级配置的详细信息。当要安装插件时。Jenkins 将动态添加所需的配置字段都安装了插件之后。



从磁盘重新加载配置

Jenkins存储所有系统，并建立作业配置细节存储在Jenkins主目录的XML文件中。这里构建的历史也都被存储。 如果要从一个Jenkins 实例迁移构建工作到另一个，或存档旧的构建工作，将需要添加或删除相应的构建工作目录到 Jenkins 的构建目录。 不需要 Jenkins 脱机来做到这一点，可以简单地使用“Reload Configuration from Disk””选项重新加载Jenkins系统，然后直接建立作业配置使用。



管理插件

在这里，人们可以安装各种各样的插件，从不同的源代码管理工具，如Git, Mercurial 或 ClearCase等第三方插件，代码质量和代码覆盖度量报告。插件可以通过管理插件屏幕安装，更新和删除。访问：http://localhost:8080/jenkins/pluginManager/ 显示结果如下：



系统信息 - System Information

该屏幕显示所有当前Java系统性能和系统环境变量的列表。在这里，人们可以准确地检查Jenkins 在Java的哪个版本正在运行，在哪些用户下运行等等。

系统日志 - System Log

系统日志屏幕是一个方便的方式来查看实时的 Jenkins 日志文件内容。此外，主要采用此屏幕可进行故障排除。

负载统计 - Load Statistics

此页面显示了Jenkins实例是如何在并发的数量方面建立图形数据和按给定的长度构建了队列，构建在执行之前需要等待一个比较的时间。 这些统计数据可以给出是否需要额外容量或额外的构建节点，从基础结构的角度来看需要一个好主意。



脚本终端 - Script Console

屏幕可让您在服务器上运行Groovy脚本。因为它需要 Jenkins 内部架构的强大的知识，可对高级故障排除。

管理结点 - Manage nodes

Jenkins能够处理并行和分布式构建。在此屏幕上，可以配置生成你想要的。Jenkins同时运行，并且，如果您正在使用分布式构建，并建立了构建节点。一个构建节点可在另一台机器Jenkins用它来执行它的构建。



关闭准备 - Prepare for Shutdown

如果有必要关闭Jenkins，或Jenkins运行在服务器上，最好不要在这个时候执行构建。要关闭 Jenkins 干净，可以用准备关机的链接，这样可以防止被启动的任何新版本。最终，当所有当前的构建已经完成，一下就能关闭 Jenkins 干净。

