pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml' // Path to your docker-compose.yml file
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub, no credentials needed since the repo is public
                git url: 'https://github.com/bharth360/my-node-app.git', branch: 'main'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Build the backend Docker image using the Dockerfile.backend
                    sh 'docker build -t my-node-app-backend -f Dockerfile.backend .'
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Build the frontend Docker image using the Dockerfile.frontend
                    sh 'docker build -t my-node-app-frontend -f Dockerfile.frontend .'
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    // Start all the containers in detached mode using docker-compose
                    sh 'docker-compose -f $DOCKER_COMPOSE_FILE up -d'
                }
            }
        }

        stage('Health Check') {
            steps {
                script {
                    // Verify that the backend is up and accessible
                    sh 'curl --fail http://localhost:3000 || exit 1'
                }
            }
        }
    }

    post {
        always {
            // Clean up and bring down the containers after the pipeline execution
            sh 'docker-compose -f $DOCKER_COMPOSE_FILE down'
        }
    }
}
