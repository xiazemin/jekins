# section2

Pipeline由多个步骤组成，允许您构建，测试和部署应用程序。Jenkins Pipeline允许您以简单的方式组合多个步骤，可以帮助您建模任何类型的自动化过程。



想像一个“一步一步”，就像执行单一动作的单一命令一样。当一个步骤成功时，它移动到下一步。当步骤无法正确执行时，Pipeline将失败。



当Pipeline中的所有步骤成功完成后，Pipeline被认为已成功执行。



Linux，BSD和Mac OS

在Linux，BSD和Mac OS（类Unix）系统上，该sh步骤用于在Pipeline中执行shell命令。



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent any

    stages {

        stage\('Build'\) {

            steps {

                sh 'echo "Hello World"'

                sh '''

                    echo "Multiline shell steps works too"

                    ls -lah

                '''

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    stage\('Build'\) {

        sh 'echo "Hello World"'

        sh '''

            echo "Multiline shell steps works too"

            ls -lah

        '''

    }

}

Windows

基于Windows的系统应该使用bat执行批处理命令的步骤。



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent any

    stages {

        stage\('Deploy'\) {

            steps {

                retry\(3\) {

                    sh './flakey-deploy.sh'

                }



                timeout\(time: 3, unit: 'MINUTES'\) {

                    sh './health-check.sh'

                }

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    stage\('Build'\) {

        bat 'set'

    }

}

超时，重试等等

有一些强大的步骤，“包装”其他步骤，可以轻松地解决问题，如重试（retry）步骤，直到成功或退出，如果步骤太长（timeout）。



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent any

    stages {

        stage\('Deploy'\) {

            steps {

                retry\(3\) {

                    sh './flakey-deploy.sh'

                }



                timeout\(time: 3, unit: 'MINUTES'\) {

                    sh './health-check.sh'

                }

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    stage\('Deploy'\) {

        retry\(3\) {

            sh './flakey-deploy.sh'

        }



        timeout\(time: 3, unit: 'MINUTES'\) {

            sh './health-check.sh'

        }

    }

}

“部署”阶段重试flakey-deploy.sh脚本3次，然后等待health-check.sh脚本执行最多3分钟。如果健康检查脚本在3分钟内未完成，则Pipeline将在“部署”阶段被标记为失败。



“包装”的步骤，例如timeout和retry可以包含其它步骤，包括timeout或retry。



我们可以组合这些步骤。例如，如果我们想重新部署我们的部署5次，但是在失败的阶段之前，总是不想花3分钟以上：



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent any

    stages {

        stage\('Deploy'\) {

            steps {

                timeout\(time: 3, unit: 'MINUTES'\) {

                    retry\(5\) {

                        sh './flakey-deploy.sh'

                    }

                }

            }

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    stage\('Deploy'\) {

        timeout\(time: 3, unit: 'MINUTES'\) {

            retry\(5\) {

                sh './flakey-deploy.sh'

            }

        }

    }

}

完成

当Pipeline完成执行后，您可能需要运行清理步骤或根据Pipeline的结果执行某些操作。可以在本post节中执行这些操作。



Jenkinsfile \(Declarative Pipeline\)

pipeline {

    agent any

    stages {

        stage\('Test'\) {

            steps {

                sh 'echo "Fail!"; exit 1'

            }

        }

    }

    post {

        always {

            echo 'This will always run'

        }

        success {

            echo 'This will run only if successful'

        }

        failure {

            echo 'This will run only if failed'

        }

        unstable {

            echo 'This will run only if the run was marked as unstable'

        }

        changed {

            echo 'This will run only if the state of the Pipeline has changed'

            echo 'For example, if the Pipeline was previously failing but is now successful'

        }

    }

}

Toggle Scripted Pipeline \(Advanced\)



Jenkinsfile \(Scripted Pipeline\)

node {

    try {

        stage\('Test'\) {

            sh 'echo "Fail!"; exit 1'

        }

        echo 'This will run only if successful'

    } catch \(e\) {

        echo 'This will run only if failed'



        // Since we're catching the exception in order to report on it,

        // we need to re-throw it, to ensure that the build is marked as failed

        throw e

    } finally {

        def currentResult = currentBuild.result ?: 'SUCCESS'

        if \(currentResult == 'UNSTABLE'\) {

            echo 'This will run only if the run was marked as unstable'

        }



        def previousResult = currentBuild.previousBuild?.result

        if \(previousResult != null && previousResult != currentResult\) {

            echo 'This will run only if the state of the Pipeline has changed'

            echo 'For example, if the Pipeline was previously failing but is now successful'

        }



        echo 'This will always run'

    }

}





