def workspace_folder = "workspace/${env.JOB_NAME}".replace('%2F', '_')


node {
  ws(workspace_folder) {
    stage('Cleanup workspace') {
      sh 'rm -rf *'
    }

    stage('Checkout') {
      checkout scm
    }

    stage('Run flake8') {
      sh "flake8"
    }
  }
}
