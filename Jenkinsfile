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
      docker.image('cdrx/pyinstaller-alpine:latest').inside {
        sh 'pyinstaller --onefile sources/add2vals.py'
        sh 'sleep 5m'
      }
    } finally {
      archiveArtifacts 'sources/dist/add2vals'
    }
  }
}