pipeline {
    agent any

    environment {
        IMAGE_NAME = "dockerhub_username/react-app"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Build') {
            steps {
                echo "Installing dependencies & building React app"
                sh '''
                  npm install
                  npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                sh '''
                  npm test -- --watch=false || true
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

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                  docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                  docker stop react-app || true
                  docker rm react-app || true
                  docker pull $IMAGE_NAME:$IMAGE_TAG
                  docker run -d \
                    --name react-app \
                    -p 80:80 \
                    $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
