pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '009160053341.dkr.ecr.us-east-1.amazonaws.com/my-app-repo'
        IMAGE_TAG = "nodejs-app-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github-ssh', url: 'git@github.com:bharth360/my-node-app.git'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh 'docker tag $ECR_REPO:$IMAGE_TAG $ECR_REPO:latest'
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                script {
                    sh 'docker push $ECR_REPO:$IMAGE_TAG'
                    sh 'docker push $ECR_REPO:latest'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'docker rmi $ECR_REPO:$IMAGE_TAG'
                    sh 'docker rmi $ECR_REPO:latest'
                }
            }
        }
    }
}
