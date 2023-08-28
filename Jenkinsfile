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
        sh '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            returntocorp/semgrep semgrep ci '''      
      }
    }
  }
}
