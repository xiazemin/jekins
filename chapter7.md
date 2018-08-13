# Pipeline 开发工具

Jenkins Pipeline包含内置文档和 Snippet Generator，它们是开发Pipeline时的关键资源。它们提供了针对当前安装的Jenkins版本和相关插件定制的详细帮助和信息。在本节中，我们将讨论可能有助于开发Jenkins Pipeline的其他工具和资源。



Blue Ocean编辑器

在 Blue Ocean Pipeline编辑器提供了一个WYSIWYG的方式来创建声明Pipeline。编辑器提供了Pipeline中所有阶段，平行分支和步骤的结构视图。编辑器会根据Pipeline更改进行验证，消除许多错误，甚至被提交。在幕后，它仍然生成声明性的Pipeline代码。



命令行Pipeline Linter

Jenkins可以在实际运行之前从命令行验证或“ lint ”声明式Pipeline。这可以使用Jenkins CLI命令或使用适当的参数进行HTTP POST请求来完成。我们建议使用 SSH接口 运行linter。有关如何正确配置Jenkins以进行安全命令行访问的详细信息，请参阅Jenkins CLI文档。



通过CLI通过SSH进行Linting

\# ssh \(Jenkins CLI\)

\# JENKINS\_SSHD\_PORT=\[sshd port on master\]

\# JENKINS\_HOSTNAME=\[Jenkins master hostname\]

ssh -p $JENKINS\_SSHD\_PORT $JENKINS\_HOSTNAME declarative-linter &lt; Jenkinsfile

通过HTTP POST使用 curl

\# curl \(REST API\)

\# Assuming "anonymous read access" has been enabled on your Jenkins instance.

\# JENKINS\_URL=\[root URL of Jenkins master\]

\# JENKINS\_CRUMB is needed if your Jenkins master has CRSF protection enabled as it should

JENKINS\_CRUMB=\`curl "$JENKINS\_URL/crumbIssuer/api/xml?xpath=concat\(//crumbRequestField,\":\",//crumb\)"\`

curl -X POST -H $JENKINS\_CRUMB -F "jenkinsfile=&lt;Jenkinsfile" $JENKINS\_URL/pipeline-model-converter/validate

例子

以下是Pipeline Linter的两个实例。第一个例子显示了linter在通过无效的输出时Jenkinsfile，缺少agent声明的一部分。



Jenkinsfile

pipeline {

  agent

  stages {

    stage \('Initialize'\) {

      steps {

        echo 'Placeholder.'

      }

    }

  }

}

Linter输出无效的Jenkins文件

\# pass a Jenkinsfile that does not contain an "agent" section

ssh -p 8675 localhost declarative-linter &lt; ./Jenkinsfile

Errors encountered validating Jenkinsfile:

WorkflowScript: 2: Not a valid section definition: "agent". Some extra configuration is required. @ line 2, column 3.

     agent

     ^



WorkflowScript: 1: Missing required section "agent" @ line 1, column 1.

   pipeline &\#125;

   ^

在第二个例子中，Jenkinsfile已经被更新为包含缺少any的agent。linter现在报告Pipeline是有效的。



Jenkinsfile

pipeline {

  agent any

  stages {

    stage \('Initialize'\) {

      steps {

        echo 'Placeholder.'

      }

    }

  }

}

Linter输出有效的Jenkins文件

ssh -p 8675 localhost declarative-linter &lt; ./Jenkinsfile

Jenkinsfile successfully validated.

“Replay”Pipeline运行与修改

通常，Pipeline将在经典的Jenkins Web UI中定义，或者通过提交到Jenkinsfile源代码控件来定义。不幸的是，这两种方法都不适用于Pipeline的快速迭代或原型设计。“重播”功能可以快速修改和执行现有流水线，而无需更改Pipeline配置或创建新的提交。



用法

要使用“重播”功能：



1、在构建历史记录中选择先前完成的运行。



Pipeline 开发工具



2、点击左侧菜单中的“重播”



Pipeline 开发工具



3、进行修改并单击“运行”。在这个例子中，我们将“ruby-2.3”更改为“ruby-2.4”。



Pipeline 开发工具



4、检查更改的结果



一旦您对更改感到满意，您可以使用Replay再次查看它们，将其复制回Pipeline作业Jenkinsfile，然后使用您通常的工程流程提交。



特征

可以在同一运行中多次调用 - 允许轻松并行测试不同的更改。

也可以在仍在进行中的Pipeline运行中调用 - 只要Pipeline包含语法正确的Groovy并且能够启动，它可以被Replayed。

参考的共享库代码也可以修改 - 如果Pipeline运行引用 共享库，共享库中的代码也将作为Replay页面的一部分显示和修改。

限制

无法重播语法错误的Pipeline运行 - 这意味着无法查看代码，无法检索其中所做的任何更改。当使用Replay进行更多重要的修改时，在将其更改保存到Jenkins之外的文件或编辑器之前，请先运行它们。

重播的Pipeline行为可能与其他方法开始的运行有所不同 - 对于不属于多分支流水线的Pipeline，对于原始运行和Replayed运行，提交信息可能不同。见JENKINS-36453

Pipeline单元测试框架

Pipeline单元测试框架是Jenkins项目不支持的第三方工具。

该Pipeline单元测试框架 可以让你的单元测试Pipeline和共享库完全运行之前。它提供了一个模拟的执行环境，真正的Pipeline的步骤是模仿对象，模仿对象



是可以使用检查期望的方式取代的。虽然新的和粗糙的边缘，但有希望。该项目的README包含示例和使用说明。





