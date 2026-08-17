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
                npm ci
                npm test
                '''
            }
        }
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.62.0-noble'
                    reuseNode true
                }
            }
            environment {
                CI = 'true'
                
            }
            steps {
                sh '''
                echo "Playwright test stage is in progress"
                npm install -g serve
                serve -s build
                '''
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
        
    }
    post {
        always{
            junit '**/test-results/**/*.xml'
        }
    }
}