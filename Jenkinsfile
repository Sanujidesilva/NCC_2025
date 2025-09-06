pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials-id' // Jenkins credentials ID
        DOCKER_HUB_REPO = 'yourdockerhubusername/your-repo-name'
        IMAGE_TAG = 'latest'
        GIT_REPO = 'https://github.com/YourUsername/YourForkedRepo.git'
    }

    stages {
        stage('Pull Git Repository') {
            steps {
                echo 'Cloning Git repository...'
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    docker.build("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Hub') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_HUB_CREDENTIALS}") {
                        docker.image("${DOCKER_HUB_REPO}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
