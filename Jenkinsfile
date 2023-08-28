pipeline {
  agent any
    environment {
      // Required for a Semgrep Cloud Platform-connected scan:
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
    }
    stages {
      // stage('Print-Vars') {
      //   steps {
      //     sh 'printenv | sort'
      //   }
      // }
      stage('semgrep-scan') {
        steps {
          sh 'pip3 install semgrep'
          sh 'semgrep ci'
        }
    }
}
