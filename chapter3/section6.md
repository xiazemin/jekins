# Jenkins远程测试

网络测试，如，selenium 测试可以通过主从和 selenium 套件插件远程安装在机器上运行。下列步骤显示了如何运行使用此配置来进行远程测试。

第1步 - 确保主从配置到位。得到主詹金斯服务器。转到 Manage Jenkins → Manage Nodes节点。



在我们的节点列表，标签为 DXBMEM30 的机器是从机。在这个例子中，主从机都是 windows 机器。



第2步 - 点击配置\(configure\)来配置DXBMEM30从机。



第3步 − 确保启动方法是把它作为“Launch slave agents via Java Web Start”。





第4步 − 现在转到从机器，并从那里，打开一个浏览器实例转到Jenkins主实例。然后转到 Manage Jenkins → Manage Nodes。转到DXBMEM30并点击它。





第5步 − 点击 DXBMEM30 实例





第6步 − 向下滚动，将看到“Launch”选项正在启动“Java Web Start”选项





第7步− 你会看到一个安全警告。点击接受复选框并单击运行。





现在，您将看到一个Jenkins从机窗口打开，现在连接上了。



第8步 − 在从服务器上配置运行您的测试。在这里必须确保所创建的任务只是专门为了运行 Selenium 测试。



在作业配置，确保选择 ‘Restrict where this project can be run’ 被选中，并在标签表达填入从机属节点的名称。





第9步 − 确保你的作业的一部分 selenium 有配置。必须确保 Sample.html 文件和selenium-server.jar 文件也存在于从机上。



遵循所有上述步骤，点击生成，该项目将运行在从机上 Selenium 按预期测试。



