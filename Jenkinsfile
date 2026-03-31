pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_USERNAME/devsecops-demo.git'
            }
        }

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh 'sonar-scanner'
            }
        }

        stage('Snyk Scan') {
            steps {
                sh 'snyk test || true'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devsecops-demo .'
            }
        }

        stage('Run App') {
            steps {
                sh 'docker run -d -p 3000:3000 devsecops-demo'
                sh 'sleep 10'
            }
        }

        stage('ZAP Scan') {
            steps {
                sh 'docker run --rm --network host ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://127.0.0.1:3000 || true'
            }
        }
    }
}
