pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        HOME_SONAR = tool 'scanner'
    }

    stages {
        stage("Check out") {
            steps {
                git branch: 'feature', url: 'https://github.com/chumaedeogu/example-voting-app.git'
            }
        }

        stage('Voting App') {
            steps {
                script {
                    dir('vote') {
                        // Static code analysis with SonarQube
                        stage("Test Static Code Analysis") {
                            steps {
                                withSonarQubeEnv('sonar') {
                                    sh """
                                    ${HOME_SONAR}/bin/sonar-scanner \
                                    -Dsonar.projectName="voteapp" \
                                    -Dsonar.projectKey="voteapp"
                                    """
                                }
                            }
                        }

                        // Wait for SonarQube Quality Gate
                        stage("Wait for Quality Gate") {
                            steps {
                                script {
                                    def result = waitForQualityGate()
                                    if (result.status != 'OK') {
                                        error "Pipeline failed due to SonarQube Quality Gate failure: ${result.status}"
                                    }
                                }
                            }
                        }

                        // Trivy File System Check
                        stage("Trivy FS Check") {
                            steps {
                                sh 'trivy --fs --severity HIGH,CRITICAL --exit-code 1 chumaedeogu/vote:${BUILD_NUMBER}'
                            }
                        }

                        // Build Docker Image
                        stage("Build Docker Image") {
                            steps {
                                sh 'docker build -t chumaedeogu/vote:${BUILD_NUMBER} .'
                            }
                        }

                        // Trivy Image Scan
                        stage("Trivy Image Check") {
                            steps {
                                sh 'trivy --image --severity HIGH,CRITICAL --exit-code 1 chumaedeogu/vote:${BUILD_NUMBER}'
                            }
                        }
                    }
                }
            }
        }
    }
}
