pipeline {
    agent any
    stages {
        stage('WithOut Docker') {
            steps {
                sh '''
                    touch no-container.txt
                    ls -la
                    whoami
                '''
            }
        }
        stage('With Docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                ls -la
                whoami
                touch yes-container.txt
                '''
            }
        }
    }
}