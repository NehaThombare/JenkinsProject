pipeline {
    agent any
    environment {
        DOCKER_USER = "your-dockerhub-username" // Replace this
        APP_NAME = "hello-python-app"
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    // Build the custom image
                    sh "docker build -t ${DOCKER_USER}/${APP_NAME}:latest ."
                    
                    // Login and Push to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-login', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push ${DOCKER_USER}/${APP_NAME}:latest"
                    }
                }
            }
        }
    }
}
