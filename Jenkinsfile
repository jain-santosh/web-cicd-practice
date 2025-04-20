pipeline {
    agent any

    tools {
        nodejs "NodeJS 18"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/<your-username>/web-cicd-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Run tests here (optional)'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy step here. Could be copy files, or FTP, or to Netlify/Vercel via API.'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
