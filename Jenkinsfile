pipeline {
    agent any
    environment {
        NPM_GLOBAL = "${env.WORKSPACE}/.npm-global"
        DOCKER_HUB_CREDENTIALS_ID = 'teche-ai-dockerhub'	
        DOCKER_IMAGE_NAME = 'techeai/techedash'
        DOCKER_IMAGE_TAG = 'latest'
    }
    stages {
            stage('Setup NPM Global Directory') {
            steps {
                script {
                    // Create a directory for npm global installs
                    sh 'mkdir -p $NPM_GLOBAL'

                    // Configure npm to use the new directory
                    sh 'npm config set prefix $NPM_GLOBAL'

                    // Add the new directory to the PATH for the current session
                    sh 'export PATH=$NPM_GLOBAL/bin:$PATH'

                    // Make the change persistent for future sessions
                    sh 'echo "export PATH=$NPM_GLOBAL/bin:$PATH" >> ~/.bashrc'
                }
            }
        }
        stage('Clone Repository') {
            steps {
                 git url: 'https://github.com/techeAI/techedash.git', branch: 'main'
                sh 'npm install -g yarn'
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
