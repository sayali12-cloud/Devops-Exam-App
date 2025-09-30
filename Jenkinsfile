pipeline {
    agent any
    
    environment {
        COMPOSE_PROJECT_NAME = "exam-app-project"
        IMAGE_NAME = "exam-app-flask"
        IMAGE_TAG = "latest"
    }

    stages {
        stage ("Git Clone") {
            steps {
                echo " Project code clone "
                git url: "https://github.com/VedTambe/Devops-Exam-App.git", branch: "main"
            }
        }

        stage ("Build Docker Image") {
            steps {
                echo "Docker image build ..."
                sh "docker compose build"
            }
        }

        stage ("Security Scan - Trivy") {
            steps {
                echo " Docker image  Trivy vulnerability scan ..."
                sh """
                trivy image ${IMAGE_NAME}:${IMAGE_TAG} || true
                """
            }
        }

        stage ("Static Code Analysis - SonarQube") {
            steps {
                echo " SonarQube  code quality check ..."
                // SonarQube Jenkins plugin requried
                withSonarQubeEnv('sonarqube-server') {
                    sh "sonar-scanner -Dsonar.projectKey=exam-app -Dsonar.sources=."
                }
            }
        }

        stage ("Project Start") {
            steps {
                echo " Project containers start ..."
                sh "docker compose up -d"
            }
        }

        stage ("Verify Containers") {
            steps {
                echo " containers verify ..."
                sh "docker ps"
            }
        }

        stage ("Filesystem Scan - Trivy") {
            steps {
                echo " Trivy filesystem + config scan ..."
                sh """
                trivy fs . || true
                """
            }
        }
    }

    post {
        always {
            echo " Pipeline complted!"
        }
        failure {
            echo " pipeline failed."
        }
    }
}
