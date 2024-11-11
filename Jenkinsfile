pipeline {
    agent any
    
    environment {
        // Set your Docker registry and image name
        DOCKER_REGISTRY = 'rhrishikesh'
        IMAGE_NAME = 'flask-app'
        GIT_REPO = 'https://github.com/R45Hrishikesh/myrepo2.git'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git branch: 'main', url: "${GIT_REPO}"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest .'
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to DockerHub (make sure Jenkins credentials are set up)
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                    
                    // Push the Docker image to DockerHub
                    sh 'docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:latest'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Docker image successfully built and pushed.'
        }
        failure {
            echo 'Build or Push failed.'
        }
    }
}
