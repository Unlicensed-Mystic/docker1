pipeline {
    agent any

    environment {
        IMAGE_NAME    = 'akshay8969/finance-tracker'
        CONTAINER_NAME = 'finance-tracker-app'
        APP_PORT      = '3000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling latest code from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/Unlicensed-Mystic/docker1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: ${IMAGE_NAME}:${BUILD_NUMBER}"
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Stopping and removing any existing container...'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm   ${CONTAINER_NAME} || true"

                echo "Starting new container on port ${APP_PORT}..."
                sh """
                    docker run -d \
                        -p ${APP_PORT}:80 \
                        --name ${CONTAINER_NAME} \
                        --restart always \
                        ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Verify') {
            steps {
                sh "docker ps --filter name=${CONTAINER_NAME} --format 'table {{.Names}}\\t{{.Status}}\\t{{.Ports}}'"
                echo "✅ Application is running at http://localhost:${APP_PORT}"
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully! Finance Tracker is live.'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above for details.'
        }
    }
}
