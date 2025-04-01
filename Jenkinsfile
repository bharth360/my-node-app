pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub
                git 'https://github.com/bharth360/my-node-app.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-node-app-backend -f Dockerfile.backend .'
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-node-app-frontend -f Dockerfile.frontend ./frontend'
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    sh 'curl --fail http://localhost:3000 || exit 1'
                }
            }
        }
    }

    post {
        always {
            // Clean up resources after the build
            sh 'docker-compose down'
        }
    }
}
