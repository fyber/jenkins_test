{
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
