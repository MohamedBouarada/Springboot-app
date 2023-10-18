node {
    git credentialsId: 'github-creds',
        url: 'https://github.com/MohamedBouarada/Springboot-app', branch: 'master'
}

pipeline {
    agent any
    environment {
        REPOSITORY_URL = 'https://github.com/MohamedBouarada/Springboot-app'
        DOCKER_HUB_REPO = 'mohamedbouarada/springbootapp'
        IMAGE_TAG = 'latest'
    }
    tools {
        // Specify the name of the Maven installation you configured in Jenkins
        maven 'Maven'
    }
    stages {
        stage('Clone repository') {
            steps {
                script {
                    checkout([$class: 'GitSCM'])
                }
            }
        }

        stage('Build and Package') {
            steps {
                // Build the application (e.g., using Maven)
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                        // Build the Docker image
                        sh "docker build -t ${env.DOCKER_HUB_REPO}:${env.IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub (you need Docker Hub credentials)
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_id', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                        withDockerRegistry([url: 'https://index.docker.io/v1/']) {
                            sh "docker logout"
                            sh 'echo $dockerHubPassword | docker login -u $dockerHubUser --password-stdin'
                            sh "docker push ${env.DOCKER_HUB_REPO}:${env.IMAGE_TAG}"
                        }
                    }
                }
            }
        }
    }
    
}
