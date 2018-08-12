# Pipeline

安装完成后，开始将Jenkins运行并创建Pipeline。

Jenkins Pipeline是一套插件，支持将连续输送Pipeline实施和整合到Jenkins。Pipeline提供了一组可扩展的工具，用于将“复制代码”作为代码进行建模。

Jenkinsfile 是一个包含Jenkins Pipeline定义的文本文件，并被检入源代码控制。这是“Pipeline代码”的基础; 处理连续输送Pipeline的一部分应用程序，以像其他代码一样进行版本检查。创建Jenkinsfile提供了一些直接的好处：

自动创建所有分支和拉请求的Pipeline

Pipeline上的代码审查/迭代

Pipeline的审计跟踪

Pipeline的唯一真实来源 ，可以由项目的多个成员查看和编辑。

虽然在Web UI或a中定义Pipeline的语法 Jenkinsfile是相同的，但通常认为最佳做法是在Jenkinsfile中定义Pipeline并检查源控制。

开始创建您的第一个Pipeline

快速入门Pipeline：



将其中一个示例复制到您的存储库并将其命名Jenkinsfile

单击Jenkins中的New Item菜单 

new-item-selection



为您的新项目提供名称（例如我的Pipeline），然后选择多分支Pipeline

单击添加源按钮，选择要使用的存储库的类型并填写详细信息。

点击保存按钮并观看您的第一条Pipeline运行！

您可能需要修改一个示例Jenkinsfile以使其与您的项目一起运行。尝试修改sh命令以运行您在本地计算机上运行的相同命令。



设置你的Pipeline后，Jenkins将自动检测在存储库中创建的任何新分支或拉请求，并为其启动运行Pipeline。



继续“运行多个步骤”



快速启动示例

以下是一些简单的复制和粘贴的一个简单的流水线与各种语言的例子。



Java

enkinsfile \(Declarative Pipeline\)

pipeline {

    agent { docker 'maven:3.3.3' }

    stages {

        stage\('build'\) {

            steps {

                sh 'mvn --version'

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

/\* Requires the Docker Pipeline plugin \*/

node\('docker'\) {

    checkout scm

    stage\('Build'\) {

        docker.image\('maven:3.3.3'\).inside {

            sh 'mvn --version'

        }

    }

}



