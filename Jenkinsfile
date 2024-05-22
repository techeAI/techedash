pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'teche-ai-dockerhub'	
        DOCKER_IMAGE_NAME = 'techeai/techedash'
        DOCKER_IMAGE_TAG = 'latest'
    }
    stages {
        stage('Clone Repository') {
            steps {
                 git url: 'https://github.com/techeAI/techedash.git', branch: 'main'
                sh 'yarn install'
                sh 'yarn build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', env.DOCKER_HUB_CREDENTIALS_ID) {
                        def image = docker.image("${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}")
                        image.push()
                    }
                }
            }
        }
    }
}
