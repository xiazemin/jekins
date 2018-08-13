# BlueOcean Pipeline运行详细信息

BlueOcean Pipeline运行详细信息视图显示与单个Pipeline运行相关的信息，并允许用户编辑或重播该运行。以下是“运行详细信息”视图部分的详细概述。



overview \(1\)



运行状态 - 此图标以及顶部菜单栏的背景颜色指示此Pipeline运行的状态。

Pipeline名称 - 此运行Pipeline的名称。

运行编号 - 此Pipeline运行的编号。Pipeline的每个分支（和拉请求）唯一的ID号。

选项卡选择器 - 查看此运行的详细选项卡。默认为“ Pipeline ”。

重新运行Pipeline - 再次执行此运行的Pipeline。

编辑Pipeline - 在Pipeline编辑器中打开此运行的Pipeline。

转到 “经典” - 切换到“经典”UI视图，了解此运行的详细信息。

关闭详细信息 - 这将关闭“详细信息”视图，并将用户返回给此Pipeline的“活动”“活动”视图。

分支或拉式请求 - 此运行的分支或拉动请求。

提交ID - 此运行的提交ID。

持续时间 - 此运行的持续时间。

完成时间 - 这个运行完成了多久。

更改作者 - 具有此运行更改的作者姓名。

选项卡视图 - 显示所选选项卡的信息。

Pipeline运行状态

BlueOcean通过更改顶部菜单栏的颜色来更容易地查看当前Pipeline运行状态：蓝色为“正在进行”，绿色为“已通过”，黄色为“不稳定”，红色为“失败“，灰色为”中止“。



特殊例子

BlueOcean针对源控制中的Pipeline进行了优化，但也可以显示其他类型项目的详细信息。BlueOcean为所有支持的项目类型提供相同的选项卡，但这些选项卡可能会显示不同的信息。



Souce Control以外的Pipeline

对于不基于源代码管理的Pipeline，BlueOcean仍显示“提交标识”，“分支”和“更改”，但这些字段留空。在这种情况下，顶部菜单栏不包括“编辑”选项。



Freestyle Projects

对于Freestyle Projects，Blue Ocean仍然提供相同的选项卡，但Pipeline选项卡仅显示控制台日志输出。“Rerun”或“Edit”选项也不会显示在顶部菜单栏中。



Matrix projects

BlueOcean不支持Matrix Projects。查看Matrix项目将重定向到该项目的“Classic UI”视图。



Tabs

“运行详细信息”视图的每个选项卡提供有关运行的特定方面的信息。



Pipeline

这是默认选项卡，并给出了此Pipeline运行流程的总体视图。它显示每个阶段和并行分支，这些阶段的步骤，以及这些步骤的控制台输出。上图概述显示了成功的Pipeline运行。如果运行过程中的特定步骤失败，则该选项卡将自动默认为从失败的步骤显示控制台日志。下图显示了一个失败的运行。



Pipeline运行详细信息视图



Changes

Pipeline运行详细信息视图



Tests

“tests”选项卡显示有关此运行的测试结果的信息。如果测试结果发布步骤（例如“发布JUnit测试结果”（junit））步骤，此选项卡将仅包含信息。如果没有记录任何结果，这个表将会说，如果所有测试通过，该选项卡将报告通过测试的总数。在故障的情况下，该选项卡将显示故障中的日志详细信息，如下所示。



Pipeline运行详细信息视图



当前一个运行失败并且当前的运行修复了这些故障时，此选项卡将记录固定的文本并显示其日志。



Pipeline运行详细信息视图



Artifacts

“Artifacts”选项卡显示使用“存档工件”（archive）步骤保存的任何工件的列表。单击列表中的项目将下载它。运行中的完整输出日志可从此列表中下载。







