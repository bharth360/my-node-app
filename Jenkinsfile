pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'your-dockerhub-username'
        DOCKER_HUB_PASSWORD = 'your-dockerhub-password'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-github-username/my-app.git'
            }
        }

        stage('Build and Push Backend') {
            steps {
                script {
                    sh "docker build -t your-dockerhub-username/my-backend:latest ./backend"
                    sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USER --password-stdin"
                    sh "docker push your-dockerhub-username/my-backend:latest"
                }
            }
        }

        stage('Build and Push Frontend') {
            steps {
                script {
                    sh "docker build -t your-dockerhub-username/my-frontend:latest ./frontend"
                    sh "docker push your-dockerhub-username/my-frontend:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "docker-compose down"
                    sh "docker-compose pull"
                    sh "docker-compose up -d"
                }
            }
        }
    }

    post {
        failure {
            mail to: 'your-email@example.com',
                 subject: 'Deployment Failed',
                 body: 'Check logs for details.'
        }
    }
}
