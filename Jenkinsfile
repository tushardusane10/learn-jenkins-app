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
                echo "Installed docker"
                node --version
                npm --version
                npm ci
                npm run build
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