pipeline {
    agent any

    environment {
        registry = "mohamedbouarada/newspringbootapp"
    }

    tools {
        // Specify the name of the Maven installation you configured in Jenkins
        maven 'Maven'
        // If you have Docker tool configured in Jenkins, uncomment the following line and specify the Docker tool name
        // docker 'DockerToolName' 
    }

    stages {
        stage('Docker Login') {
            steps {
                script {
                    // Authenticate with Docker Hub using your credentials
                    withDockerRegistry([credentialsId: 'dockerhub_id']) {
                        // This stage will log in to Docker Hub
                        sh 'docker login -u <your-dockerhub-username> -p <your-dockerhub-password>'
                    }
                }
            }
        }
        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub_id') {
                        // Add the BUILD_NUMBER to the image tag
                        def customImage = docker.build("${registry}:${env.BUILD_NUMBER}", "--build-arg JAR_FILE=target/*.jar")
                        customImage.push()
                    }
                }
            }
        }

        stage('Cleaning up') {
            steps {
                // Use double quotes for string interpolation
                sh "docker rmi ${registry}:${env.BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed 🙁'
        }
        unstable {
            echo 'Pipeline is unstable 😕'
        }
        changed {
            echo 'Pipeline has changes!'
        }
    }
}