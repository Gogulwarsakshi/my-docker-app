pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Code cloned successfully'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Test') {
            steps {
                echo 'Tests running'
            }
        }
    }
}
