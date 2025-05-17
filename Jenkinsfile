pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('0fe9be08-5a5d-4bc1-8915-cd4bd3f43acf') // ID from Jenkins credentials
        IMAGE_NAME = 's7edgedocker/java-docker-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/satish3143/java-docker-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${env.IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully to Docker Hub."
        }
        failure {
            echo "Build or push failed."
        }
    }
}
