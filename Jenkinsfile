pipeline {
    agent any
    environment {
        // Replace 'your-dockerhub-username' with your actual Docker Hub ID
        DOCKER_HUB_USER = 'nehathombare' 
        IMAGE_NAME = 'hello-python-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CREDS = 'docker-hub-creds' 
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NehaThombare/JenkinsProject.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // This builds using the Dockerfile in your repo
                    app = docker.build("${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDS) {
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }
        stage('Update Manifest') {
            steps {
                script {
                    // Using withCredentials to push back to GitHub securely
                    withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                        sh """
                            # Update the image line in your deployment file
                            sed -i 's|image:.*|image: ${DOCKER_HUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}|g' deployment.yaml
                            
                            git config user.email "jenkins@example.com"
                            git config user.name "Jenkins CI"
                            git add deployment.yaml
                            git commit -m "Update image to version ${IMAGE_TAG} [skip ci]"
                            
                            # Push using the token for authentication
                            git push https://${GIT_USER}:${GIT_PASS}@github.com/NehaThombare/JenkinsProject.git main
                        """
                    }
                }
            }
        }
    }
}
