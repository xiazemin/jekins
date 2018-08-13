# Pipeline

Jenkins Pipeline是一套插件，支持将连续输送Pipeline实施和整合到Jenkins。Pipeline提供了一组可扩展的工具，用于通过PipelineDSL为代码创建简单到复杂的传送Pipeline。

通常，此“Pipeline代码”将被写入 Jenkinsfile项目的源代码控制存储库，例如：

Jenkinsfile \(Declarative Pipeline\)

pipeline {

```
agent any 



stages {

    stage\('Build'\) { 

        steps { 

            sh 'make' 

        }

    }

    stage\('Test'\){

        steps {

            sh 'make check'

            junit 'reports/\*\*/\*.xml' 

        }

    }

    stage\('Deploy'\) {

        steps {

            sh 'make publish'

        }

    }

}
```

}

agent 表示Jenkins应该为Pipeline的这一部分分配一个执行者和工作区。

stage 描述了这条Pipeline的一个阶段。

steps 描述了要在其中运行的步骤 stage

sh 执行给定的shell命令

junit是由JUnit插件提供的 用于聚合测试报告的Pipeline步骤。

为什么是Pipeline？

Jenkins从根本上讲是一种支持多种自动化模式的自动化引擎。Pipeline在Jenkins上添加了一套强大的自动化工具，支持从简单的连续集成到全面的连续输送Pipeline的用例。通过建模一系列相关任务，用户可以利用Pipeline 的许多功能：

代码：Pipeline以代码的形式实现，通常被检入源代码控制，使团队能够编辑，审查和迭代其传送流程。

耐用：Pipeline可以在计划和计划外重新启动Jenkins管理时同时存在。

Pausable：Pipeline可以选择停止并等待人工输入或批准，然后再继续Pipeline运行。

多功能：Pipeline支持复杂的现实世界连续交付要求，包括并行分叉/连接，循环和执行工作的能力。

可扩展：Pipeline插件支持其DSL的自定义扩展 以及与其他插件集成的多个选项。

虽然Jenkins一直允许基本形式的自由式工作联合起来执行顺序任务，Pipeline使这个概念成为Jenkins的最好的一个部分。

基于Jenkins的核心可扩展性，Pipeline也可以由Pipeline共享库用户和插件开发人员扩展。

下面的流程图是在Jenkins Pipeline中容易建模的一个连续发货方案的示例：

Pipeline 介绍

![](/assets/importpip.png)

Pipeline 条件

Step

单一任务，从基础中告诉了Jenkins应该怎么做。例如，要执行shell命令，请make使用以下sh步骤：sh 'make'。当插件扩展Pipeline DSL时，通常意味着插件已经实现了一个新的步骤。

Node

Pipeline执行中的大部分工作都是在一个或多个声明node步骤的上下文中完成的。将工作限制在Node步骤中有两件事情：

通过将项目添加到Jenkins队列来调度要运行的块中包含的步骤。一旦执行器在节点上空闲，步骤就会运行。

创建工作区（特定于该特定Pipeline的目录），可以从源代码控制中检出的文件完成工作。

根据您的Jenkins配置，某些工作空间在一段时间不活动后可能无法自动清除。

Stage

stage是定义整个Pipeline的概念上不同子集的一个步骤，例如：“Build”，“Test”和“Deploy”，许多插件用于可视化或呈现Jenkins Pipeline状态/进度。

Jenkins Pipeline是一套插件，支持将连续输送Pipeline实施和整合到Jenkins。Pipeline 提供了一组可扩展的工具，用于通过Pipeline DSL为代码创建简单到复杂的传送Pipeline 。 



本节介绍Jenkins Pipeline的一些关键概念，并帮助介绍在运行的Jenkins实例中定义和使用Pipelines的基础知识。



先决条件

要使用Jenkins Pipeline，您将需要：



Jenkins 2.x或更高版本（旧版本回到1.642.3可能会工作，但不推荐）

Pipeline插件

要了解如何安装和Pipeline插件，请参阅管理插件。



Pipeline 定义

脚本Pipeline是用Groovy写的 。Groovy语法的相关位将在本文档中根据需要进行介绍，因此，当了解Groovy时，不需要使用Pipeline。



可以通过以下任一方式创建基本Pipeline：



直接在Jenkins网页界面中输入脚本。

通过创建一个Jenkinsfile可以检入项目的源代码管理库。

用任一方法定义Pipeline的语法是一样的，但是Jenkins支持直接进入Web UI的Pipeline，通常认为最佳实践是在Jenkinsfile Jenkins中直接从源代码控制中加载Pipeline。



在Web UI中定义Pipeline

要在Jenkins Web UI中创建基本Pipeline，请按照下列步骤操作：



单击Jenkins主页上的New Item。

Pipeline 入门







输入Pipeline的名称，选择Pipeline，然后单击确定。

Jenkins使用流水线的名称在磁盘上创建目录。包含空格的管道名称可能会发现不希望路径包含空格的脚本中的错误。

Pipeline 入门



在脚本文本区域中，输入Pipeline，然后单击保存。

Pipeline 入门



单击立即生成以运行Pipeline。

Pipeline 入门



单击“构建历史记录”下的＃1，然后单击控制台输出以查看Pipeline的完整输出。



Pipeline 入门



上面的示例显示了在Jenkins Web UI中创建的基本Pipeline的成功运行，使用两个步骤。



Jenkinsfile \(Scripted Pipeline\)

node { 

    echo 'Hello World' 

}

：node 在Jenkins环境中分配一个执行器和工作空间。



：echo 在控制台输出中写入简单的字符串



在SCM中定义管道

复杂的Pipeline难以在Pipeline配置页面的文本区域内进行写入和维护。为了使这更容易，Pipeline也可以写在文本编辑器中，并检查源控件，作为Jenkinsfile，Jenkins可以通过Pipeline脚本从SCM选项加载的控件。



为此，在定义Pipeline时，从SCM中选择Pipeline脚本。



选择SCM选项中的Pipeline脚本后，不要在Jenkins UI中输入任何Groovy代码; 您只需指定要从其中检索Pipeline的源代码中的路径。更新指定的存储库时，只要Pipeline配置了SCM轮询触发器，就会触发一个新构建。



文本编辑器，IDE，GitHub等将使用Groovy代码进行语法高亮显示， 第一行Jenkinsfile应该是\#!/usr/bin/env groovy Jenkinsfile。

内置文档

Pipeline配有内置的文档功能，可以更轻松地创建不同复杂性的Pipeline。根据Jenkins实例中安装的插件自动生成和更新内置文档。



内置文档可以在全局范围内找到： localhost：8080 / pipeline-syntax /，假设您有一个Jenkins实例在本地端口8080上运行。同样的文档也作为管道语法链接到任何配置的Pipeline的侧栏中项目。



Pipeline 入门



代码段生成器

内置的“Snippet Generator”实用程序有助于为单个步骤创建一些代码，发现插件提供的新步骤，或为特定步骤尝试不同的参数。



Snippet Generator动态填充Jenkins实例可用的步骤列表。可用的步骤数量取决于安装的插件，它明确地暴露了在Pipeline中使用的步骤。



要使用代码段生成器生成步骤代码片段：



从配置的流水线或本地主机：8080 / pipeline-syntax导航到Pipeline语法链接（上面引用）。

在“ 样品步骤”下拉菜单中选择所需的步骤

使用“ 样品步骤”下拉列表下方的动态填充区域配置所选步骤。

单击生成Pipeline脚本以创建一个可以复制并粘贴到Pipeline中的Pipeline代码段。

Pipeline 入门



要访问有关所选步骤的其他信息和/或文档，请单击帮助图标（由上图中的红色箭头指示）。



全局变量引用

除了代码片段生成器之外，Pipeline还提供了一个内置的“ 全局变量引用”。像Snippet Generator一样，它也是由插件动态填充的。与代码段生成器不同的是，全局变量引用仅包含Pipeline提供的变量的文档，这些变量可用于Pipeline。



在Pipeline中默认提供的变量是：



ENV

脚本化Pipeline可访问的环境变量，例如： env.PATH或env.BUILD\_ID。请参阅内置的全局变量参考 ，以获取管道中可用的完整和最新的环境变量列表。



PARAMS

将为Pipeline定义的所有参数公开为只读 地图，例如：params.MY\_PARAM\_NAME。



currentBuild

可用于发现有关当前正在执行的Pipeline信息，与如属性currentBuild.result，currentBuild.displayName等等请教内置的全局变量引用 了一个完整的，而且是最新的，可用的属性列表currentBuild。



进一步阅读

本节只是划伤了Jenkins Pipeline可以做的工作，但应该为您提供足够的基础，开始尝试使用测试Jenkins实例。



在下一节中，Jenkinsfile将会更多的管道步骤与实现成功的，真实的Jenkins Pipeline的模式一起讨论。



其他资源

Pipeline步骤参考，包含分布在Jenkins更新中心的插件提供的所有步骤。

Pipeline示例，一个社区策划的可复制Pipeline示例的集合。

