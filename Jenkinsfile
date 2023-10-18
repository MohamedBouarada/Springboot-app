pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code source depuis GitHub
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                // Construire l'image Docker
                script {
                    docker.build("scrape-master:latest")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                // Poussez l'image Docker vers DockerHub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        docker.image("scrape-master:latest").push()
                    }
                }
            }
        }
    }
}