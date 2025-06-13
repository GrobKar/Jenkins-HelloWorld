pipeline {
    agent any
    environment {
        KUBERNETES_CREDENTIALS_ID = 'kubeconfig'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: "${KUBERNETES_CREDENTIALS_ID}"]) {
                        sh '''
                            kubectl apply -f deployment.yaml
                            kubectl apply -f service.yaml
                        '''
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Successfully deployed to Kubernetes!'
        }
        failure {
            echo 'Deployment failed. Check logs for errors.'
        }
    }
}
