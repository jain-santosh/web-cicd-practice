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
    
            echo '🧹 Attempting to stop any previous http-server process on port 5000...'
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                bat '''
                for /f "tokens=5" %%a in ('netstat -aon ^| findstr :5000') do taskkill /F /PID %%a
                '''
            }
    
            echo '🚀 Starting local HTTP server...'
            bat 'cmd /c start "" http-server build -p 5000'
        }
        failure {
            echo '❌ Build failed. Check console output.'
        }
    }
}
