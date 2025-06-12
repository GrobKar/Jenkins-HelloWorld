pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = 'grobkardev/jenkins-docker-task' // DockerHub repository
        DOCKER_CREDENTIALS_ID = 'DockerHub'  // Jenkins credentials ID
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
                    bat '''
                    docker build -t %DOCKER_HUB_REPO%:latest .
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS_ID}",
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD')]) {

                        bat '''
                        echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                        docker push %DOCKER_HUB_REPO%:latest
                        '''
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
