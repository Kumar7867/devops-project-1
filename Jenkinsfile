pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning source code from GitHub'
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image'
                sh '''
                  docker build -t devops-project:1.0 .
                '''
            }
        }

        stage('Docker Deploy') {
            steps {
                echo 'Running Docker container'
                sh '''
                  docker rm -f devops-project-container || true
                  docker run -d \
                    --name devops-project-container \
                    -p 8085:80 \
                    devops-project:1.0
                '''
            }
        }
    }
}

