node {
  stage('Build') {
    docker.image('python:2-alpine').inside {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  stage('Test') {
    try {
      docker.image('qnib/pytest').inside {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
    } finally {
      always {  
        junit 'test-reports/results.xml'
      }
    }
  }
  stage ('Deliver') {
    try {
      docker.image('cdrx/pyinstaller-linux:python2').inside {
        sh 'pyinstaller --onefile sources/add2vals.py'
      }
    } finally {
      sh 'sleep 5m'
      archiveArtifacts 'dist/add2vals'
    }
  }
}