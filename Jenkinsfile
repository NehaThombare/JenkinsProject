pipeline {
    agent {
        docker { image 'python:3.9-slim' } 
    }
    stages {
        stage('Build') {
            steps {
                echo 'Preparing the environment...'
            }
        }
        stage('Run Script') {
            steps {
                sh 'python hello.py'
            }
        }
    }
}
