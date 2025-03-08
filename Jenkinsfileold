pipeline {
    agent { label '!ansible' }
    stages {
        stage('Check Environment') {
            steps {
                // Print environment variables to check if Docker is in PATH
                bat 'set'
            }
        }
        stage('Clean Workspace') {
            steps {
                deleteDir() // Deletes the contents of the workspace
            }
        }
        stage('Clone Repository') {
            steps {
               
                git url: 'https://github.com/chumaedeogu/example-voting-app.git', branch: 'main'
            }
        }
        stage('docker compose') {
            steps {
                script{
               
                bat 'docker compose -f docker-compose.yml up -d'
                }
            }
        }
    }
}
