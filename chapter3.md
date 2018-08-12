# Jenkins指标与趋势

有各种插件这些都是 Jenkins 用以展示进行了一段时间构建度量。这些指标是用于了解您的构建，以及它们如何故障/通过一段时间。作为一个例子，让我们来看看“Build History Metrics plugin”。

这个插件一旦安装了计算以下度量全部建立：

平均无故障时间 \(MTTF\)

平均恢复时间 \(MTTR\)

生成时间的标准偏差

第1步- 转到 Jenkins 仪表板，然后单击管理Jenkins

第2步 - 转到管理插件\(Manage Plugins\)选项。

第3步- 转到“Available”选项卡，并搜索插件“Build History Metrics plugin”，并选择“install without restart”。

第4步- 下面的屏幕显示出来，以确认成功安装了插件。重新启动 Jenkins 实例。

当转到工作页面，你会看到计算的度量表。度量显示有过去的7天，最近30天，所有的时间。

要在Jenkins中看到总体趋势，也有可从内部构建和Jenkins插件搜集资料，并以图形格式显示它们。这里的插件的一个实例是“Hudson global-build-stats plugin'。所以，让我们将一步步演示。

第1步- 转到 Jenkins 仪表板，然后单击管理Jenkins

第2步- 转到管理插件\(Manage Plugins\)选项

第3步 - 转到可用\(Available\)选项卡，并搜索插件“Hudson global-build-stats plugin”并选择“install without restart”。

第4步- 下面的屏幕显示出来，以确认成功安装插件。重新启动 Jenkins 实例。

要看到全局统计数据，请按照步骤5至8。

第5步- 转到Jenkins 仪表板，然后单击管理Jenkins。在管理Jenkins屏幕，向下滚动，现在，你现在会看到一个名为“Global Build Stats”的选项。点击这个链接。

第6步 - 点击按钮“Initialize stats”。这里做的事情是，它收集的所有现有已经被执行的记录和可以根据这些结果来创建构建的图表。

第7步 - 一旦数据被初始化，它现在就创建一个新的图表。点击“Create new chart”链接。

第8步 - 弹出一个输入相关新图表的详细信息。输入以下必填信息

Title – 任何标题的信息，对于本实施例我们填写 “Demo”

Chart Width – 800

Chart Height – 600

Chart time scale – Daily

Chart time length – 30 days

信息的其余部分可以保持原样。输入信息完成后，请单击创建新表\(Create New chart\)。

现在，您将看到它显示构建的趋势随时间变化的图表。

![](/assets/importTab.png)

如果您单击图表中的任何部分，它会给你一个作业的细节和构建信息。

