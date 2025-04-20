pipeline {
    agent any

    tools {
        nodejs 'NodeJS 23' // This name must match what's configured in Jenkins -> Global Tool Configuration
    }

    environment {
        REPO_URL = 'https://github.com/jain-santosh/web-cicd-practice.git'
        BUILD_DIR = 'build'
        PORT = '5000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing node modules...'
                bat 'npm install'
            }
        }

        stage('Build App') {
            steps {
                echo 'Building the React app...'
                bat 'npm run build'
            }
        }

        stage('Archive Build') {
            steps {
                echo 'Archiving build artifacts...'
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully.'

            // Kill any existing http-server running on port 5000
            echo '🧹 Stopping any previous instance of HTTP server on port 5000...'
            bat 'for /f "tokens=5" %%a in (\'netstat -aon ^| findstr :5000\') do taskkill /F /PID %%a'

            // Start the new HTTP server
            echo "🚀 Starting local HTTP server at http://localhost:${PORT}..."
            bat "start http-server ${BUILD_DIR} -p ${PORT}"
        }
        failure {
            echo '❌ Build failed. Check console output.'
        }
    }
}
