pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "exam-app-project"
    }

    stages {

        stage("Git Clone") {
            steps {
                echo "Cloning project from GitHub..."
                git url: "https://github.com/VedTambe/Devops-Exam-App.git", branch: "main"
            }
        }

        stage("Docker Image Build") {
            steps {
                echo "Building Docker images..."
                sh "docker compose build"
            }
        }

        stage("Start Application (Deploy)") {
            steps {
                echo "Starting application containers..."
                sh "docker compose up -d"
            }
        }

        stage("Verify Containers") {
            steps {
                echo "Checking running containers..."
                sh "docker ps"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Please check logs."
        }
    }
}
