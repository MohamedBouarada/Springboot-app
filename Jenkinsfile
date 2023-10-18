pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_id')
    }
    tools {
        // Specify the name of the Maven installation you configured in Jenkins
        maven 'Maven'
    }
    stages {
        stage('Checkout from GitHub') {
            steps {
                // Checkout the source code from GitHub
                checkout scm
            }
        }
        stage('Build and Package') {
            steps {
                // Build the application (e.g., using Maven)
                sh 'mvn clean package'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Authenticate with Docker Hub using credentials
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKERHUB_CREDENTIALS', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD']]) {
                        // Build a Docker image
                        def dockerImage = docker.build("mohamedbouarada/newspringbootapp:${env.BUILD_ID}")

                        // Push the Docker image to Docker Hub
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
