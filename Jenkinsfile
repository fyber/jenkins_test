node {
    // Need to replace the '%2F' used by Jenkins to deal with / in the path (e.g. story/...)
    // because tests that do getResource will escape the % again, and the test files can't be found.
    // See https://issues.jenkins-ci.org/browse/JENKINS-34564 for more.
    ws("workspace/${env.JOB_NAME}/${env.BRANCH_NAME}".replace('%2F', '_')) {
        stage 'Cleanup workspace'
        sh 'chmod 777 -R .'
        sh 'rm -rf *'

        stage 'Checkout SCM'
        checkout scm

        stage 'Install & Unit Tests'
            timestamps {
                timeout(time: 30, unit: 'MINUTES') {
                    try {
                        sh 'pip install . -U --pre'
                        sh 'python setup.py nosetests --with-xunit'
                    } finally {
                        step([$class: 'JUnitResultArchiver', testResults: 'nosetests.xml'])
                    }
                }
            }

        stage 'Build wheels'
            sh 'python setup.py bdist_wheel'

        stage 'Archive build artifacts'
            archive 'dist/*'
    }
}
