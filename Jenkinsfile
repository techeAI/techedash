node {
  stage 'Checkout code'
  git 'https://github.com/techeAI/techedash.git'
  stage 'Docker build'
  if (env.BRANCH_NAME == 'main') {
    docker.build('techedash')
    docker.withRegistry('https://index.docker.io/v1/') {
        docker.image('techedash').push('latest')
    }
  }       
}
