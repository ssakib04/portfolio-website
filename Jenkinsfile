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
    }
}
