# BlueOcean

BlueOcean重新考虑了Jenkins的用户体验。BlueOcean由Jenkins Pipeline设计，但仍然兼容自由式工作，减少了团队成员的混乱，增加了清晰度。

连续交付（CD）Pipeline的复杂可视化，允许快速和直观地了解Pipeline的状态。

Pipeline编辑器通过引导用户直观和可视化的过程创建Pipeline，使创建Pipeline平易近人。

个性化，以适应团队每个成员的角色需求。

需要干预和/或出现问题时确定精度。BlueOcean显示了Pipeline需要注意的地方，便于异常处理和提高生产率。

用于分支和拉取请求的本地集成可以在GitHub和Bitbucket中与其他人进行代码协作时最大限度提高开发人员的生产力。

要开始使用BlueOcean，请参阅BlueOcean入门。

FAQ

为什么存在BlueOcean？

世界已经从纯粹功能的开发人员工具转移到开发人员工具，成为“开发人员体验”的一部分。也就是说，它不再是一个单一的工具，而是开发人员在一天中使用的许多工具，以及它们如何协同工作，以实现对开发人员有益的工作流程 - 这是开发人员体验。

像Heroku，Atlassian和Github这样的开发人员工具公司已经提出了一些被认为是开发人员的经验的酒吧，开发人员越来越期待卓越的设计。近年来，开发人员正在变得越来越快地被吸引到不仅可以实现功能的工具，而且被设计为无缝地融入其工作流程中，并且是使用的乐趣。这种转变代表了Jenkins需要提升的设计和用户体验标准。

创建和可视化连续输送Pipeline对于许多Jenkins用户来说是有价值的，这在社区为满足他们的需求而创建的5+个插件中得到证明。对我们来说，这意味着需要重新审视Jenkins目前如何表达这些概念，并考虑将输送Pipeline作为Jenkins用户体验的中心主题。

这不仅仅是持续的交付概念，而且是开发人员每天使用的工具--Github，Bitbucket，Slack，HipChat，Puppet或Docker。它不仅仅是Jenkins，而是围绕着Jenkins跨越多种工具的开发人员工作流程。

新团队没有时间学习组装自己的Jenkins经验 - 他们希望通过更快地运送更好的软件来改善他们的上市时间。组装理想的Jenkins经验是我们可以一起工作，作为Jenkins用户和贡献者界定的社区。随着时间的推移，开发人员对良好用户体验的期望将会改变，Blue Ocean的使命将使Jenkins项目得以回应。

Jenkins社区已经将汗水和泪水浇灌到现有的最具技术能力和可扩展性的软件自动化工具中。今天没有任何改变Jenkins开发人员经验的事情，只是邀请别人 - 封闭的来源 - 来做这件事。

名字起源

BlueOcean名称来自 蓝海战略 ，而不是在有争议的空间中看待战略问题，而是考虑更大的无争议的空间中的问题。为了更简单的说，考虑冰球传奇韦恩格雷茨基的这个报价：“滑冰到冰球将在哪里，而不是在那里”。

BlueOcean支持自由式工作吗？

Blue Ocean旨在为Pipeline提供丰富的体验，并与您在系统中配置的任何自由式作业兼容。但是，它们将无法从为Pipeline建立的任何功能中受益 - 例如，Pipeline可视化。

由于BlueOcean设计是可扩展的，社区将来有可能将其扩展到其他工作类型。

这对于经典的Jenkins UI来说意味着什么？

其意图是，随着BlueOcean成熟，用户回到现有UI的原因将越来越少。

例如，在第一个版本中，我们将主要针对Pipeline作业。您可能可以在Blue Ocean中看到现有的非Pipeline作业，但可能无法从新UI中配置它们一段时间。这意味着用户必须跳回到经典的用户界面来配置非Pipeline作业。

这可能会有更多的例子，这就是为什么经典的用户界面在长期来看仍然是重要的。

这对我的插件意味着什么？

可扩展性是Jenkins的一个非常核心的概念，所以能够延伸BlueOceanUI是很重要的。基于一些研究，我们制定了一种方式，允许&lt;ExtensionPoint name=..&gt;在BlueOcean的标记中使用，为插件贡献UI（插件可以拥有自己的BlueOcean扩展点，就像今天在Jenkins一样） 。BlueOcean本身（至今为止）使用这些扩展点实现。扩展程序通常由插件提供，只要他们希望为BlueOcean经验做出贡献，他们将有一些额外的JavaScript来提供扩展。

目前正在使用哪些技术？

BlueOcean作为Jenkins插件本身构建。然而，有一个关键的区别。它为http请求提供了自己的端点，并通过不同的路径提供html / javascript，而没有现有的Jenkins UI标记/脚本。React.js和ES6用于提供Blue Ocean的JavaScript组件。受到这个优秀的开源项目（反应插件）的启发，建立了一个&lt;ExtensionPoint&gt;模式，允许扩展来自任何Jenkins插件（仅使用Javascript），如果它们无法加载，则会发生故障。

在哪里可以找到源代码？

源代码可以在Github上找到：

[BlueOcean](https://github.com/jenkinsci/blueocean-plugin)

[Jenkins设计语言](https://github.com/jenkinsci/jenkins-design-language)

安装

BlueOcean可以安装在现有的Jenkins环境中，也可以使用Docker进行运行 。



要开始在现有的Jenkins环境中使用Blue Ocean插件，它必须运行Jenkins 2.7.x或更高版本：



登录到您的Jenkins服务器

单击边栏中的管理Jenkins然后管理插件

选择可用的选项卡，并使用搜索栏查找BlueOcean

单击“安装”列中的复选框

单击安装而不重新启动或立即下载并重新启动后安装

BlueOcean入门



有关如何安装和Pipeline插件的深入描述，请参阅“ Pipeline插件”部分。



大多数Blue Ocean在安装后不需要额外的配置。现有管道和作业将继续照常运行。但是，Blue Ocean将首次创建或添加Pipeline，将要求访问您的存储库（Git或GitHub）的权限，以便根据这些存储库创建Pipeline。



与Docker

Jenkins项目每次发布Blue Ocean的新版本时，都会发布一个内置BlueOcean的Docker容器。该jenkinsci/blueocean 图像基于当前的Jenkins长期支持 （LTS）版本，并且已经准备就绪。



预先安装一个新的Jenkins：BlueOcean



确保Docker已安装。

跑 docker run -p 8888:8080 jenkinsci/blueocean:latest

浏览到本地主机：8888 /蓝色

可以使用与Jenkins项目发布的其他图像相同的配置选项来配置Blue Ocean容器 。



开始BlueOcean

一旦Jenkins环境安装了Blue Ocean，用户可以通过点击Jenkins网络用户界面的导航栏中的Open Blue Ocean开始使用Blue Ocean 。或者，用户可以直接浏览BlueOcean/blue，例如Jenkins环境的 URL http://JENKINS\_URL/blue。



BlueOcean入门



如果Pipeline已经存在于当前的Jenkins实例上，则会显示 Blue Ocean Dashboard。



如果这是一个新的Jenkins实例，BlueOcean将展示一个盒子，以“ Create a new pipeline”。



BlueOcean入门



导航栏

BlueOcean在大多数BlueOcean的顶部使用一个通用的导航栏。它包括五个按钮：



Jenkins - 导航到仪表板（重新加载，如果已经查看）

Pipeline - 导航到仪表板（如果已经查看，则不执行任何操作）

管理 - 管理此Jenkins实例（使用Classic UI）

切换到“经典”UI - 切换到“经典” Jenkins UI

注销 - 注销当前用户，返回到Jenkins登录页面

使用标准导航栏的视图将在其下方添加另一个与该视图相关的选项。一些视图用一个特别适合该视图的通用导航栏来代替。



切换到“Classic”UI

BlueOcean可能不支持某些用户需要的遗留或管理功能。对于那些希望退出BlueOcean的用户来说，“退出”图标位于BlueOcean大部分页面的顶部。点击退出图标将导航到BlueOcean中当前页面的“经典”页面中最相关的页面。



BlueOcean入门



BlueOcean中的一些链接，如管理，也将导航到经典的网页界面，当没有BlueOcean的时候。在这些情况下，BlueOcean将根据需要自动将用户带入经典的网页界面。

