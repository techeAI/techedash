pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'teche-ai-dockerhub'	
        DOCKER_IMAGE_NAME = 'techeai/techedash'
        DOCKER_IMAGE_TAG = 'latest'
        DATE_TAG = new Date().format('yyyyMMdd-HHmmss')
    }
    stages {
        stage('Clone Repository') {
            steps {
                 git url: 'https://github.com/techeAI/techedash.git', branch: 'main'
                sh 'cp .env.example .env'
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
                        def dateimage = docker.image("${env.DOCKER_IMAGE_NAME}:${env.DATE_TAG}")
                        dateimage.push()
                    }
                }
            }
        }
    }
}
