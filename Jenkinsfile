node {
    checkout scm
    docker.image('python:2-alpine').inside {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
        }
    }

    docker.image('qnib/pytest').inside {
        stage('Test') {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }

    stage('Manual Approval') {
        input(message: "Lanjutkan ke tahap Deploy?")
    }

    docker.image('cdrx/pyinstaller-linux:python2').inside("""-u root --entrypoint=''""") {
        stage('Deploy') {
            sh '/root/.pyenv/shims/pyinstaller --onefile sources/add2vals.py'
            sh 'sleep 60'
            archiveArtifacts 'dist/add2vals'
        }
    }
}
