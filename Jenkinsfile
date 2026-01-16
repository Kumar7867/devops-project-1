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

        stage('Cleanup Old Images') {
    steps {
        sh '''
          echo "Cleaning up old Docker images (keeping last 3)..."

          docker images ${IMAGE_NAME} \
            --format "{{.Tag}}" \
            | grep -E '^[0-9]+$' \
            | sort -n \
            | grep -v "^${IMAGE_TAG}$" \
            | head -n -2 \
            | xargs -r -I {} docker rmi ${IMAGE_NAME}:{}
        '''
    }
}

    }
}

