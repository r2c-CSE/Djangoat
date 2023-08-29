pipeline {
  agent any
  environment {
    // Required for a Semgrep Cloud Platform-connected scan:
    SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
    // Set repo name to expected format
    SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*)$/, '$1')
  }
  stages {
    stage('Print-Vars') {
      steps {
        sh 'printenv | sort'
      }
    }
    stage('semgrep-diff-scan') {
      when {
        branch "PR-*"
      }
      // Ideally want to transform this to the merge-base output of CHANGE_BRANCH and CHANGE_TARGET
      steps {
        sh '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
            -e SEMGREP_BASELINE_REF="master" \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            returntocorp/semgrep semgrep ci '''      
      }
    }
    stage('semgrep--scan') {
      when {
        branch "master"
      }
      steps {
        sh '''docker pull returntocorp/semgrep && \
            docker run \
            -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
            -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
            -v "$(pwd):$(pwd)" --workdir $(pwd) \
            returntocorp/semgrep semgrep ci '''      
      }
    }
  }
}
