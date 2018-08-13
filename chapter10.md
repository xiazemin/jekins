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

BlueOcean Pipeline编辑器是任何人开始在Jenkins创建Pipeline的最简单的方法。这也是现有Jenkins用户开始采用Pipeline的好方法。



编辑器允许用户创建和编辑声明性Pipeline，添加可以同时运行的阶段和并行化任务，这取决于他们的需要。完成后，编辑器将管道保存为源代码存储库Jenkinsfile。如果Pipeline需要再次更换，Blue Ocean可以轻松地跳回到可视化编辑器中以随时修改Pipeline。



Pipeline编辑



开始编辑

要使用编辑器，用户必须首先 在Blue Ocean中创建Pipeline， 或者已在Jenkins中创建一个或多个现有Pipeline。如果编辑现有Pipeline，则该Pipeline的凭据必须允许将更改推送到目标资源库。



编辑器可以通过以下方式启动：



仪表板“New Pipeline”按钮

活动视图单次运行

Pipeline运行详细信息

限制

仅基于SCM的声明性Pipeline

证书必须具有写入权限

与声明式Pipeline没有完全一致

Pipeline重新排序和注释已删除

导航栏

Pipeline编辑器包括顶部的标准导航栏，下面有一个本地导航栏。本地导航栏包括：



Pipeline名称 - 这将包括分支依赖或如何

取消 - 放弃对Pipeline进行的更改。

保存 - 打开保存Pipeline对话框。

Pipeline设置

默认情况下，编辑器的右侧显示“Pipeline Settings”。可以通过单击Stage编辑器 中不是Stage或“Add Stage”按钮x之一的任何位置来访问此工作表



Agent

“代理”部分控制Pipeline将使用什么代理。这与“代理”指令相同。



环境

“环境”部分允许我们为Pipeline设置环境变量。这与“环境”指令相同。



Stage editor

左侧编辑器屏幕包含Stage编辑器，用于创建Pipeline的阶段。



Pipeline编辑



可以通过单击" button to the right of an existing stage. Parallel stages can be added by clicking the "现有Stage下方的“ ”按钮将阶段添加到Pipeline。可以使用Stage配置表中的上下文菜单删除阶段。



Stage编辑器将在设置完毕后显示每个Stage的名称。包含不完整或无效信息的阶段将显示警告符号。Pipeline在编辑时可能会出现验证错误，但在修复错误之前无法保存。



Pipeline编辑



Stage配置

在Stage编辑器中选择Stage将打开右侧的“Stage配置”工作表。在这里，我们可以更改Stage的名称，删除Stage，并向Stage添加步骤。



Pipeline编辑



Stage的名称可以设置在Stage配置表的顶部。上下文菜单（右上角三个点）可用于删除当前阶段。单击“添加步骤”将显示可用步骤类型的列表，其顶部有搜索栏。可以使用步骤配置表中的上下文菜单删除步骤。添加步骤或选择现有步骤将打开步骤配置表。



Pipeline编辑



步骤配置

从Stage配置表中选择一个步骤将打开“步骤Stage”页。



Pipeline编辑



此工作表将根据步骤类型而有所不同，包含所需的任何字段或控件。步骤的名称不能更改。上下文菜单（右上方的三个点）可用于删除当前步骤。包含不完整或无效信息的字段将显示警告符号。Pipeline在编辑时可能会出现验证错误，但在修复错误之前无法保存。



Pipeline编辑



保存Pipeline对话框

为了运行，Pipeline的更改必须保存在源代码控制中。“保存Pipeline”对话框控制对源代码控制的更改的保存。



Pipeline编辑



更改的有用描述可以添加或留空。该对话框还支持保存更改相同的分支或输入新的分支以保存到。点击“保存并运行”将保存对Pipeline的任何更改作为新的提交，将根据这些更改启动新的Pipeline运行，并将导航到此Pipeline的 活动视图。





