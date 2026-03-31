pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mohamedzaki999/devsecops-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Snyk Scan') {
            steps {
                sh 'snyk test || true'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devsecops-demo:latest .'
            }
        }

        stage('Run App') {
            steps {
                sh 'docker rm -f devsecops-demo-container || true'
                sh 'docker run -d --name devsecops-demo-container -p 3000:3000 devsecops-demo:latest'
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
