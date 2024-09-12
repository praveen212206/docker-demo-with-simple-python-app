pipeline {
    agent any
    
    environment {
        // Define environment variables if needed
        DOCKER_IMAGE = 'my-python-app'
        DOCKER_TAG = 'latest'
        REPO_URL = 'https://github.com/yourusername/your-repo.git'
        DOCKERFILE_PATH = './Dockerfile'
        APP_PORT = '8081'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git url: "${env.REPO_URL}", branch: 'main'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh """
                        docker build -t ${env.DOCKER_IMAGE}:${env.DOCKER_TAG} -f ${env.DOCKERFILE_PATH} .
                    """
                }
            }
        }
        
        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container
                    sh """
                        docker stop ${env.DOCKER_IMAGE} || true
                        docker rm ${env.DOCKER_IMAGE} || true
                    """
                    
                    // Run the Docker container
                    sh """
                        docker run -d --name ${env.DOCKER_IMAGE} -p ${env.APP_PORT}:80 ${env.DOCKER_IMAGE}:${env.DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images to save space
            sh "docker system prune -af"
        }
    }
}
