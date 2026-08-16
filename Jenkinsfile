pipeline {
    agent any

    stages {
        stage('Clean Up') {
            steps {
                sh '''echo "Cleaning up workspace"'''
                cleanWs()
                checkout scm
            }
        }
        stage('Build') {
            agent{
                docker{
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo "Installed docker"
                node --version
                npm --version
                npm ci
                npm run build
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            environment {
                CI = 'true'
                INDEX_FILE_PATH = 'build/index.html'
            }
            steps {
                sh '''
                echo "Test stage is in progress"
                test -f "$INDEX_FILE_PATH"
                npm test
                '''
            }
        }
    }
    post {
        always{
            junit '**/test-results/**/*.xml'
        }
    }
}