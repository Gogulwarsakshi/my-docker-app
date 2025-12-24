pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sakshigogul/my-docker-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t myapp:latest .
                docker tag myapp:latest $DOCKER_IMAGE:latest
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                docker push $DOCKER_IMAGE:latest
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Docker image successfully pushed to Docker Hub"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}

