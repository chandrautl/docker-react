pipeline {
    agent { docker { image 'node:16' } } // Use Node.js image for the entire pipeline

    environment {
        // Replace with Docker registry credentials ID
        DOCKER_CREDENTIALS_ID = 'da09b43e-2cec-4582-9f02-779eb83d2a09'
        // Replace with your Docker image repository name
        IMAGE_NAME = 'chandrautl/react'
        // The branch to build from (e.g., 'main', 'develop')
        GIT_BRANCH = 'master'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from Git
                git branch: "${env.GIT_BRANCH}", url: 'https://github.com/chandrautl/docker-react.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
				script {
					docker.image('node:16').inside {
                        sh 'npm install'
            }
        }
        
        stage('Build React App') {
            steps {
				script {
					docker.image('node:16').inside {
					// Build the React app for production
						sh 'npm run build'
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
                        // Push the Docker image to the registry
                        docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            echo "Build and deployment completed successfully."
        }
        failure {
            echo "Build failed."
        }
    }
}
