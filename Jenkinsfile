pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: '660eee35-418b-4eab-99ba-925622bfe994', url: 'git@github.com:dino-chiio/fastapi-cicd-pipeline.git'
            }
        }
        stage('SonarQube analysis') {
            steps {
                script {
                    // Assuming you have installed 'SonarQube Scanner' with the tool name 'SonarScanner'
                    def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv(installationName: 'SonarScanner') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    dockerImage = docker.build("fastapi-hello-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('http://localhost:5000') {
                        dockerImage.push("fastapi-hello-app:${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }
}