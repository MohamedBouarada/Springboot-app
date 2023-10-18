pipeline {
    agent any

    environment {
        registry = "mohamedbouarada/newspringbootapp"
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
                // Build the application (e.g., using Maven)
                sh 'mvn clean package'
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
}
                    }
                }
            }
        stage('Cleaning up') {
            steps{
            sh "docker rmi $registry:$BUILD_NUMBER"
        }
        }
    }
}
