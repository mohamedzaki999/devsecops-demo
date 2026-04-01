pipeline {
    agent any

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

        stage('SonarQube Scan') {
            steps {
                sh '''
                sonar-scanner \
                -Dsonar.projectKey=devsecops-demo \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://192.168.119.129:9000 \
                -Dsonar.login=sqa_2385315ca6318b3c334a0562a029cf70423a1752
                '''
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
                sh 'docker run -d --name devsecops-demo-container -p 3001:3000 devsecops-demo:latest'
                sh 'sleep 10'
            }
        }

        stage('ZAP Scan') {
            steps {
                sh 'docker run --rm --network host ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://127.0.0.1:3001 || true'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh 'minikube image load devsecops-demo:latest'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
