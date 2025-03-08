pipeline{
    agent any
    stages{
        stage("check out"){
            steps{
                git branch: 'feature', url: 'https://github.com/chumaedeogu/example-voting-app.git'
            }
        }
    stage('voting app'){
        steps{
            script{
                dir('vote'){
                load 'Jenkinsfile'
                }
            }
        }
    }
    }
}