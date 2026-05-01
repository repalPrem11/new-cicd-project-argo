pipeline {
    agent any

    environment {
        IMAGE_NAME = "premrepal/cafe-ui"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/repalPrem11/new-cicd-project-argo.git'
            }
        }

        stage('Build Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Push Image') {
            steps {
                echo "Pushing Docker image..."
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE_NAME:$TAG
                    '''
                }
            }
        }
    }
}