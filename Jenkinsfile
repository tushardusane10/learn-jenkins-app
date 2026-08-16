pipeline {
    agent any
    stages {
        stage('Clean Up') {
            steps {
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
                
            }
        }
    }
    post {
        success{
            archiveArtifacts artifacts: '**/*.jar', fingerprint: true
        }
    }
}