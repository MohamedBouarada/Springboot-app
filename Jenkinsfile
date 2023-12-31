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
                    docker.withRegistry('', 'dockerhub') {
                        // Add the BUILD_NUMBER to the image tag
                        def customImage = docker.build("${registry}:${env.BUILD_NUMBER}")
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory.ini ansible/deploy.yml'
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