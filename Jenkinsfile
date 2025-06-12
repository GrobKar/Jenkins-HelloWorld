pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = 'grobkardev/jenkins-docker-task' // Replace with your exact DockerHub repository
        DOCKER_CREDENTIALS_ID = 'dockerhub'               // Jenkins credential ID for DockerHub
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_HUB_REPO}:latest .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Use DockerHub credentials securely
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Login to DockerHub and push the built image
                        sh '''
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            docker push ${DOCKER_HUB_REPO}:latest
                        '''
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Docker image built and pushed to DockerHub successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
