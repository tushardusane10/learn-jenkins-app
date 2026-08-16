pipeline {
    agent any
    stages {
        stage('Clean Up') {
            steps {
                sh '''echo "Cleaning up workspace"'''
                cleanWs()
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
                echo "Insalled docker"
                docker --version
                node --version
                npm --version
                '''
            }
        }
    }
    post {
        success{
            archiveArtifacts artifacts: '**/*.jar', fingerprint: true
        }
    }
}