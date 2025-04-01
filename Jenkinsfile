pipeline {
    agent any
    environment {
        FRONTEND_IMAGE = 'my-node-app-frontend'
        BACKEND_IMAGE = 'my-node-app-backend'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Backend') {
            steps {
                script {
                    // Build the backend image
                    sh 'docker build -t ${BACKEND_IMAGE} -f Dockerfile.backend .'
                }
            }
        }
        stage('Build Frontend') {
            steps {
                script {
                    // Build the frontend image
                    sh 'docker build -t ${FRONTEND_IMAGE} -f Dockerfile.frontend .'
                }
            }
        }
        stage('Docker Compose Up') {
            steps {
                script {
                    // Bring up the services using docker-compose
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('Health Check') {
            steps {
                script {
                    // Add your health check logic here (optional)
                    echo 'Checking the health of the services...'
                }
            }
        }
        stage('Teardown') {
            steps {
                script {
                    // Clean up services
                    sh 'docker-compose down'
                }
            }
        }
    }
}
