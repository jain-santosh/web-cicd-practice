pipeline {
    agent any

    tools {
        nodejs 'NodeJS 23' // This name must match what's configured in Jenkins -> Global Tool Configuration
    }

    environment {
        REPO_URL = 'https://github.com/<your-username>/<your-repo>.git' // Replace with your repo
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
        }
        failure {
            echo '❌ Build failed. Check console output.'
        }
    }
}
