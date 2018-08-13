# Pipeline

Jenkins Pipeline是一套插件，支持将连续输送Pipeline实施和整合到Jenkins。Pipeline提供了一组可扩展的工具，用于通过PipelineDSL为代码创建简单到复杂的传送Pipeline。 



通常，此“Pipeline代码”将被写入 Jenkinsfile项目的源代码控制存储库，例如：



Jenkinsfile \(Declarative Pipeline\)

pipeline {

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











图1.Pipeline流量



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





