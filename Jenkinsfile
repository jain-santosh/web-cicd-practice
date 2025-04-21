pipeline {
    agent any

    tools {
        nodejs 'NodeJS 23'
    }

    environment {
        REPO_URL = 'https://github.com/jain-santosh/web-cicd-practice.git'
        IMAGE_NAME = 'react-app'
        CONTAINER_NAME = 'react-app-container'
        HOST_PORT = '5000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🛠 Building Docker image...'
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🧹 Removing old container (if any)...'
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    bat "docker rm -f %CONTAINER_NAME%"
                }

                echo '🚀 Starting new container...'
                bat "docker run -d -p %HOST_PORT%:80 --name %CONTAINER_NAME% %IMAGE_NAME%"
            }
        }
    }

    post {
        success {
            echo '✅ Docker container deployed successfully.'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
