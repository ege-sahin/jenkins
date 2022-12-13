pipeline {
    agent any
    environment{
        EXECUTE_RUN = false
        EXECUTE_TEST = false
    }

    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('checkout') {
            steps {
                checkout scm: [$class: 'GitSCM',
                    userRemoteConfigs: [[url: 'https://github.com/ege-sahin/jenkins.git']],
                                    branches: [[name: 'refs/heads/master']]
                ], poll: true
            }
        }
        stage('Build') { 
            steps {
                echo "build"
                sh '''
                    cd /home/egesahin/Desktop/jenkins
                    javac hello.java
                    '''
            }
            post{
                success{
                    script{
                        EXECUTE_RUN = true   
                    }
                }
            }
        }
        stage('run') {
            when{
                expression{
                    EXECUTE_RUN == true
                }
            }
            steps {
                echo "run"
                sh '''
                    cd /home/egesahin/Desktop/jenkins
                    java HelloWorld
                    '''
            }
            post{
                success{
                    script{
                        EXECUTE_TEST = true   
                    }
                }
            }
        }
        stage('test') {
            when{
                expression{
                    EXECUTE_TEST == false
                }
            }
            steps {
                echo "test" 
            }
        }
    }
    post {
        always{
            emailext attachLog: true, body: 'build log attached', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'
        }
    }
}
