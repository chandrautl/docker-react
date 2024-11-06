pipeline {
    agent {
        docker { image 'node:16' }
    }
    environment {
        DOCKER_CREDENTIALS_ID = '49797223-5f64-4bc2-beb9-40644954b28c' // Replace with your Docker registry credentials ID
        IMAGE_NAME = 'chandrautl/react'
        GIT_BRANCH = 'master'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.GIT_BRANCH}", url: 'https://github.com/chandrautl/docker-react.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Run React App') {
            steps {
                sh "docker run -d -p 3000:3000 ${IMAGE_NAME}:${env.BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo "React app is successfully built, pushed, and deployed."
        }
        failure {
            echo "Build or deployment failed."
        }
    }
}
