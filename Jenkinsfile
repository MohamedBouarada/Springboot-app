pipeline {
    agent any

    environment {
        registry = 'mohamedbouarada/newspringbootapp'
        registryCredential = 'dockerhub_id'
        dockerImage = ''
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

        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
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
