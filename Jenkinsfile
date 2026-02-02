pipeline {
    agent any

    tools {
        nodejs 'node18'
    }

    environment {
        ACR_REGISTRY = "bonydevopsacr.azurecr.io"
        IMAGE_NAME = "nodejs-devops-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
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
        docker build --platform linux/amd64 -t $ACR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG .
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

        stage('Deploy to AKS') {
    steps {
        sh '''
        export KUBECONFIG=/root/.kube/config
        kubectl get nodes
        kubectl set image deployment/nodejs-deployment \
        nodejs-container=$ACR_REGISTRY/$IMAGE_NAME:$IMAGE_TAG
        '''
            }
        }
    }

    post {
        success {
            echo '✅ Build, Push & Deploy Successful!'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}

