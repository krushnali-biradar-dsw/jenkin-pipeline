pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('simple-node-app:latest')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('simple-node-app:latest').run('-p 3000:3000')
                }
            }
        }
    }
}