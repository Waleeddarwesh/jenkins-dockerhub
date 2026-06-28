pipeline {
    agent any

    environment {
        APP_NAME = 'web-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                """
            }
        }

        stage('Remove Old Container') {
            steps {
                sh """
                    docker rm -f ${APP_NAME} || true
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                    docker run -d \
                    --name ${APP_NAME} \
                    -p 5000:5000 \
                    ${APP_NAME}:${BUILD_NUMBER}

                    docker ps
                """
            }
        }
    }

    post {
        success {
            echo "Docker container started successfully."
        }

        failure {
            echo "Pipeline failed."
        }

        always {
            sh "docker image prune -f || true"
        }
    }
}
