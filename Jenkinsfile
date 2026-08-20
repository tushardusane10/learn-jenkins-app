pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = 'bc0531b3-f69f-4e70-b958-ff1dfbe111aa'
        NETLIFY_AUTH_TOKEN = '$jenkins'
    }
    stages {
        stage("CLear") {
            steps {
                sh '''
                    echo " Stage clear"
                    echo "$NETLIFY_SITE_ID"
                    echo "$NETLIFY_AUTH_TOKEN"
                '''
                cleanWs()
            }    
        }
        stage("Testng"){
            parallel {
                stage("Unit Testing"){
                    agent {
                        docker{
                            image 'node:20-alpine'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                            echo "Unit test in progress"
                            npm ci
                            npm run test
                        '''
                    }
                }
                stage("Playwrite Testing"){
                    agent {
                        docker{
                            image 'mcr.microsoft.com/playwright:v1.55.0-noble'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                            echo "Playwrite test in progress"
                            npm ci
                            npx playwright test
                        '''
                    }
                }
            }
        }
        stage("Build") {
            agent{
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Unit test in progress"
                        npm ci
                        npm run build
                '''
            }
        }
        stage("Deploy") {
            
            steps {
                sh '''
                    echo "Deployment is starting"
                '''
            }
        }
    }
}