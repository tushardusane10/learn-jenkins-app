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
        stage('Testing'){
            parallel {
                    stage('Unit Testing') {
                        agent {
                            docker {
                                image 'node:20-alpine'
                                reuseNode true
                            }
                        }
                    }
                    environment {
                        CI = 'true'
                    }
                    steps {
                        sh '''
                        echo "Test stage is in progress"
                        npm ci
                        npm test
                        '''
                    }
                    post {
                        always{
                            junit '**/test-results/**/*.xml'
                        }
                    }
                }
                stage('E2E Testing') {
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
                        npm install serve
                        node_modules/serve-index/bin/serve-index.js build
                        '''
                    }
                }
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
}