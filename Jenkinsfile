pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-secret-id')
        IMAGE_NAME = 'shafat04/portfolio-image'
        TAG = 'latest'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    app = docker.build("${env.IMAGE_NAME}:${env.TAG}")
                }
            }
        }
        stage('Docker Hub Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.image("${env.IMAGE_NAME}:${env.TAG}").push()
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Stop and remove existing container if running (ignore errors if not)
                    sh 'docker stop portfolio-container || true'
                    sh 'docker rm portfolio-container || true'
                    
                    // Pull the latest image explicitly from Docker Hub
                    sh 'docker pull ${env.IMAGE_NAME}:${env.TAG}'
                    
                    // Run the newly pulled container on port 80
                    sh 'docker run -d -p 80:80 --name portfolio-container ${env.IMAGE_NAME}:${env.TAG}'
                }
            }
        }
    }
}
