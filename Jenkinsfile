pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'bharathms98'  // Your Docker Hub username
        DOCKER_HUB_PASSWORD = 'Bharath@98'  // Docker Hub password
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your GitHub repository
                git 'https://github.com/bharathms98/my-node-app.git'  // Replace with your GitHub repository
            }
        }

        stage('Build and Push Backend') {
            steps {
                script {
                    // Build and push backend Docker image to Docker Hub
                    sh "docker build -t bharathms98/my-backend:latest ./backend"
                    sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USER --password-stdin"
                    sh "docker push bharathms98/my-backend:latest"
                }
            }
        }

        stage('Build and Push Frontend') {
            steps {
                script {
                    // Build and push frontend Docker image to Docker Hub
                    sh "docker build -t bharathms98/my-frontend:latest ./frontend"
                    sh "docker push bharathms98/my-frontend:latest"
                }
            }
        }

        stage('Build and Push Nginx') {
            steps {
                script {
                    // Build and push nginx Docker image to Docker Hub
                    sh "docker build -t bharathms98/my-nginx:latest ./nginx"
                    sh "docker push bharathms98/my-nginx:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Pull the latest images and restart services
                    sh "docker-compose down"
                    sh "docker-compose pull"
                    sh "docker-compose up -d"
                }
            }
        }
    }

    post {
        failure {
            // Send failure notification email if the pipeline fails
            mail to: 'bharateshshanavad@gmail.com',  // Your email address
                 subject: 'Deployment Failed',
                 body: 'Check logs for details.'
        }
    }
}
