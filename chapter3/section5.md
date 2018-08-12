# Jenkins备份插件

Jenkins 有一个备份插件，它可以用来与Jenkins备份重要的配置设置。按照下面给出有一个适当的备份所需的步骤。

第1步- 点击Manage Jenkins，然后选择“Manage Plugins”选项。



第2步 - 在可用选项卡上，搜索“Backup Plugin”。点击“Install without Restart”。完成后，重新启动Jenkins实例







第3步 - 现在，当你转到Manage Jenkins，向下滚动，将会看到“Backup Manager”作为一个选项。点击该选项。



第4步 - 点击设置\(Setup\)。



第5步 - 在这里，主要字段是定义备份的目录。 确保是在另外一个驱动器，这个驱动器不同于 Jenkins 实例所在的位置。点击保存\(Save\)按钮。





第6步- 点击备份管理器屏幕上的“Backup Hudson configuration”来启动备份。



下一个屏幕将显示备份的状态



若要从备份中恢复，进入备份管理器屏幕上，点击 Restore Hudson configuration。



备份的列表将显示，点击一个合适的备份，然后点击Launch Restore 来开始恢复。



