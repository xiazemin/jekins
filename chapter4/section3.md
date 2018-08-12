# section3

该 agent指令告诉Jenkins在哪里以及如何执行Pipeline或其子集。如您所料，agent所有Pipeline都是必需的。



在引擎盖下，有几件事情agent会发生：



该块中包含的所有步骤均排队等待Jenkins执行。一旦执行者可用，步骤就会开始执行。

将分配一个工作区，其中将包含从源代码管理检出的文件以及Pipeline的任何其他工作文件。

有几种方法可以定义代理在Pipeline中使用，对于本次巡视，我们将仅关注使用短暂的Docker container。



Pipeline设计用于容易地使用Docker图像和容器在里面运行。这允许Pipeline定义所需的环境和工具，而无需手动配置各种系统工具和对代理的依赖。这种方法允许您几乎可以使用可以打包在Docker容器中的任何工具 。



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent {

        docker { image 'node:7-alpine' }

    }

    stages {

        stage\('Test'\) {

            steps {

                sh 'node --version'

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    /\* Requires the Docker Pipeline plugin to be installed \*/

    docker.image\('node:7-alpine'\).inside {

        stage\('Test'\) {

            sh 'node --version'

        }

    }

}

当Pipeline执行时，Jenkins将自动启动指定的容器并执行其中定义的步骤：



\[Pipeline\] stage

\[Pipeline\] { \(Test\)

\[Pipeline\] sh

\[guided-tour\] Running shell script

+ node --version

v7.4.0

\[Pipeline\] }

\[Pipeline\] // stage

\[Pipeline\] }

混合和匹配不同容器或其他代理时，在执行Pipeline时可以有很大的灵活性，有关更多配置选项，请继续使用“使用环境变量”。

