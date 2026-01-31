pipeline {
    agent any

    tools {
        nodejs 'node18'
    }

    environment {
        ACR_REGISTRY = "bonydevopsacr.azurecr.io"
        IMAGE_NAME = "nodejs-devops-app"
        IMAGE_TAG = "1.0"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $ACR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push to ACR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'acr-creds',
                                 usernameVariable: 'USERNAME',
                                 passwordVariable: 'PASSWORD')]) {

                    sh '''
                    echo $PASSWORD | docker login $ACR_REGISTRY -u $USERNAME --password-stdin
                    docker push $ACR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Docker image built and pushed successfully!'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}

