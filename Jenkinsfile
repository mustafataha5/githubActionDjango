pipeline {
    //agent any
    agent{
        node{
            label: "docker_agent_python_with_docker"
        }
    }

    environment {
        DOCKER_HUB_USERNAME = 'mustafataha5'
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials') // Docker Hub credentials
        IMAGE_NAME = 'my-django'
        IMAGE_TAG = 'jenkins'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/mustafataha5/githubActionDjango.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using the credentials
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        // Push the image to Docker Hub
                        docker.image("${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}").push()
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
            echo "Build or Push failed."
        }
    }
}
