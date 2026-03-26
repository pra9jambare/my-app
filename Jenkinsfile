pipeline {
    agent any
    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO  = '937749309122.dkr.ecr.us-east-1.amazonaws.com/my-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pra9jambare/my-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $ECR_REPO:$IMAGE_TAG ./my-app'
                }
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REPO
                    docker push $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                script {
                    sh '''
                    aws eks update-kubeconfig --name demo-cluster --region $AWS_REGION
                    helm upgrade --install my-app ./helm-chart \
                        --set image.repository=$ECR_REPO \
                        --set image.tag=$IMAGE_TAG
                    '''
                }
            }
        }
    }
    post {
        success {
            echo "Deployment to EKS successful!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}