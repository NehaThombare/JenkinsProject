pipeline {
    // This removes the need for a separate Dockerfile
    agent {
        docker { 
            image 'python:3.9-slim' 
        }
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls your code from https://github.com/NehaThombare/JenkinsProject.git
                checkout scm
            }
        }

        stage('Execute Script') {
            steps {
                // Since the agent is already Python, we just run the command
                sh 'python hello.py'
            }
        }
    }
}
