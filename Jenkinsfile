pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Prashali04/Personal_Finance_Tracker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t finance-secure-app .'
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                sh '''
                trivy image --exit-code 1 --severity CRITICAL,HIGH finance-secure-app
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop finance || true
                docker rm finance || true
                docker run -d --name finance -p 5000:5000 finance-secure-app
                '''
            }
        }
    }
}

