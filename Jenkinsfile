pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'mustafataha5'
        IMAGE_NAME = 'my-django'
        IMAGE_TAG = 'jenkins'
        AWS_INSTANCE_IP = '13.60.62.5'
       SSH_CREDENTIALS_ID = credentials('AWS-SSH-Credentials') // The ID of your SSH credentials in Jenkins
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
                    // Build the Docker image using shell commands
                    sh """
                    docker build -t ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerUser', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                    script {
                        // Login to Docker Hub using the credentials
                        sh """
                        echo '${DOCKERHUB_CREDENTIALS_PSW}' | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                        """
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image using shell commands
                    sh """
                    docker push ${DOCKER_HUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
        
        
        //  stage('SSH to EC2 and Deploy') {
        //     steps {
        //         sshagent([SSH_CREDENTIALS_ID]) {
        //             script {
        //                 // SSH into EC2 and perform tasks
        //                 sh """
        //                 ssh -i SSH_CREDENTIALS_ID_PWS  -o StrictHostKeyChecking=no ubuntu@${AWS_INSTANCE_IP} << EOF
        //                 # Add your commands to stop old container and run the new one
        //                 sudo docker stop my_old_container || true
        //                 sudo docker rm my_old_container || true
        //                 sudo docker run -d --name my_new_container my-django:jenkins
        //                 EOF
        //                 """
        //             }
        //         }
        //     }
        // }
        
        
        
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
