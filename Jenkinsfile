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
      sh '''
        mkfifo pipe
        tee flake8_warnings.txt < pipe &
        flake8 > pipe || true
        test $? -eq 0 && echo "pylint exited with $?"'''
    }

    step([
      $class: 'WarningsPublisher',
      parserConfigurations: [[
        parserName: 'Pep8',
        pattern: 'flake8_warnings.txt',
      ]],
      unstableTotalAll: '0',
      usePreviousBuildAsReference: true
    ])
  }
}
