
pipeline {
    agent any
    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "my-app"
        IMAGE_TAG = "latest"
        AWS_CREDENTIALS = "aws-ecr-credentials"
        ACCOUNT_ID = "861276077917"
        ECR_URL = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/nayub007/centralgit.git'
            }
        }
        stage('Login to Amazon ECR') {
            steps {
                script {
                    withAWS(credentials: AWS_CREDENTIALS, region: AWS_REGION) {
                        sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_URL'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $ECR_REPO:$IMAGE_TAG ."
            }
        }
        stage('Tag Docker Image') {
            steps {
                sh "docker tag $ECR_REPO:$IMAGE_TAG $ECR_URL:$IMAGE_TAG"
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker push $ECR_URL:$IMAGE_TAG"
            }
        }
    }
}
