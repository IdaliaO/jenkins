pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'git'  // ID de credencial en Jenkins
        IMAGE_NAME = 'amazonlinux'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/IdaliaO/jenkins.git'
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKERHUB_USERNAME', 
                    passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh '''
                        echo "$DOCKERHUB_PASSWORD" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
                        docker build -t "$DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG" .
                        docker push "$DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG"
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
    }
}
