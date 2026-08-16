pipeline {
    agent any
    environment {
        INDEX_FILE_PATH = 'build/index.html'
    }
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
            steps {
                sh '''echo "Test stage is in progress"
                ls -la
                npm run test
                if [ -f "$INDEX_FILE_PATH" ]; then
                    echo "$INDEX_FILE_PATH exists"
                else
                    echo "$INDEX_FILE_PATH does not exist"
                    exit 1
                fi
                '''
            }
        }
    }
    // post {
    //     success{
    //         archiveArtifacts artifacts: '**/*.jar', fingerprint: true
    //     }
    // }
}