pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-project"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        CONTAINER  = "devops-project-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                  docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Deploy') {
            steps {
                sh '''
                  docker rm -f ${CONTAINER} || true
                  docker run -d \
                    --name ${CONTAINER} \
                    -p 8085:80 \
                    ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}

