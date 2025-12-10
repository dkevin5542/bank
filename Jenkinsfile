pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Deploy with Docker Compose') {
            steps {
                echo "Stopping existing containers (if any)..."
                bat """
                cd %WORKSPACE%
                docker-compose -f %COMPOSE_FILE% down || exit /b 0
                """

                echo "Building and starting containers..."
                bat """
                cd %WORKSPACE%
                docker-compose -f %COMPOSE_FILE% up -d --build
                """
            }
        }

        stage('Show Running Containers') {
            steps {
                bat "docker ps"
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Frontend: http://localhost:3000  Backend: http://localhost:9090"
        }
        failure {
            echo "Deployment failed check the console log for Docker errors."
        }
    }
}
