pipeline {
    agent any

    environment {
        registry = "mohamedbouarada/newspringbootapp"
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
                // Use the 'sh' step to execute Maven with the configured Maven tool
                sh 'mvn clean package'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build a Docker image and push it to Docker Hub
                    docker.build(registry, "--build-arg JAR_FILE=target/*.jar")
                    docker.withRegistry('', credentialsId: 'dockerhub_id') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
