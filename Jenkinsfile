
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'hassss/sales-system:v2'
        DEPLOYMENT_NAME = 'sales-v2'
    }

    stages {
        stage('Checkout V2') {
            steps {
                checkout scm
                bat 'git branch --show-current'
            }
        }

        stage('Install Python Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'python -m pytest -v'
            }
        }

        stage('Build V2 Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Push V2 to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_TOKEN'
                    )
                ]) {
                    bat 'powershell -NoProfile -Command "$env:DH_TOKEN | docker login -u $env:DH_USER --password-stdin; if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }"'
                    bat 'docker push %IMAGE_NAME%'
                }
            }
        }

        stage('Deploy V2 to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/v2.yaml'
                bat 'kubectl set image deployment/sales-v2 sales=%IMAGE_NAME%'
                bat 'kubectl rollout status deployment/sales-v2 --timeout=180s'
            }
        }
    }

    post {
        success {
            echo 'V2 build, tests, Docker push and deployment succeeded!'
        }

        failure {
            echo 'V2 pipeline failed. Check the Console Output.'
        }
    }
}

