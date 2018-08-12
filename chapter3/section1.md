# Jenkins服务器维护

以下是一些基本的活动，一些是对 Jenkins 服务器维护的最佳实践

URL选项

在 Jenkins 实例 URL 有以下命令将开展对Jenkins实例的相关动作。

http://localhost:8080/jenkins/exit − 关闭jenkins



http://localhost:8080/jenkins/restart − 重启jenkins



http://localhost:8080/jenkins/reload − 重新加载配置



备份 Jenkins 主页

Jenkins主目录是什么，不是是在驱动器上的位置，Jenkins存储的所有信息工作，构建等等。 home目录的位置，可以点击Manage Jenkins → Configure system 看到。



设置Jenkins具有最大的可用磁盘空间分区 – 由于Jenkins 采用源代码定义各项工作，并做持续构建，所以需要始终确保Jenkins 建立具有足够的硬盘空间的驱动器上。如果你的硬盘空间已用完，那么所有建立在Jenkins实例启动将会失败。



另一个最佳实践是写 cron 作业或维护任务，可以开展清理工作，以避免Jenkins设置磁盘全部占用\(占满\)。



