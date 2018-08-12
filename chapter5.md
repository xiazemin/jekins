# Jenkins 部署

最基本的连续交付流程将至少具有三个阶段，这些阶段应在以下内容中定义Jenkinsfile：构建，测试和部署。

对于本节，我们将主要关注部署阶段，但应该注意的是，稳定的构建和测试阶段是任何部署活动的重要前身。

Jenkinsfile \(Declarative Pipeline\)

pipeline {

```
agent any

stages {

    stage\('Build'\) {

        steps {

            echo 'Building'

        }

    }

    stage\('Test'\) {

        steps {

            echo 'Testing'

        }

    }

    stage\('Deploy'\) {

        steps {

            echo 'Deploying'

        }

    }

}
```

}

Toggle Scripted Pipeline \(Advanced\)

Jenkinsfile \(Scripted Pipeline\)

node {

```
stage\('Build'\) {

    echo 'Building'

}

stage\('Test'\) {

    echo 'Testing'

}

stage\('Deploy'\) {

    echo 'Deploying'

}
```

}

阶段作为部署环境

一个常见的模式是扩展级别以捕获额外的部署环境，如“分段”或“生产”，如下面的代码段所示。

stage\('Deploy - Staging'\) {

```
steps {

    sh './deploy staging'

    sh './run-smoke-tests'

}
```

}

stage\('Deploy - Production'\) {

```
steps {

    sh './deploy production'

}
```

}

在这个例子中，我们假设我们的./run-smoke-tests脚本运行的任何“烟雾测试” 都足以将释放资格或验证到生产环境。自动将代码自动部署到生产的这种Pipeline可以被认为是“持续部署”的实现。虽然这是一个崇高的理想，但对许多人来说，连续部署可能不实际的原因很好，但仍然可以享受持续交付的好处。 Jenkins Pipeline很容易支撑两者。

要求人力投入进行

通常在阶段之间，特别是环境阶段之间，您可能需要人为的输入才能继续。例如，判断应用程序是否处于“促进”到生产环境的状态。这可以通过input步骤完成。在下面的示例中，“真实检查”阶段实际上阻止输入，如果没有人确认进度，则不会继续进行。

Jenkinsfile \(Declarative Pipeline\)

pipeline {

```
agent any

stages {

    /\* "Build" and "Test" stages omitted \*/



    stage\('Deploy - Staging'\) {

        steps {

            sh './deploy staging'

            sh './run-smoke-tests'

        }

    }



    stage\('Sanity check'\) {

        steps {

            input "Does the staging environment look ok?"

        }

    }



    stage\('Deploy - Production'\) {

        steps {

            sh './deploy production'

        }

    }

}
```

}

Toggle Scripted Pipeline \(Advanced\)

Jenkinsfile \(Scripted Pipeline\)

node {

```
/\* "Build" and "Test" stages omitted \*/



stage\('Deploy - Staging'\) {

    sh './deploy staging'

    sh './run-smoke-tests'

}



stage\('Sanity check'\) {

    input "Does the staging environment look ok?"

}



stage\('Deploy - Production'\) {

    sh './deploy production'

}
```

}

结论

这个导读旨在向您介绍使用Jenkins和Jenkins Pipeline的基础知识。因为它是非常可扩展的，Jenkins可以进行修改和配置，以处理几乎任何方面的自动化。

