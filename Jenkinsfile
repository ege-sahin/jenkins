pipeline {
    agent any 
    
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
            post {
                always{
                    emailext attachLog: true, body: 'build log attached', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'
                }
            }
        }
        stage('run') { 
            steps {
                echo "run"
                sh '''
                    cd /home/egesahin/Desktop/jenkins
                    java HelloWorld
                    '''
            }
        }
        stage('test') { 
            steps {
                echo "test" 
            }
        }
    }
}
