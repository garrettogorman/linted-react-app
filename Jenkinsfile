pipeline {
  agent any

  stages {

    stage('Setup') {
      agent {
        docker {
          image 'node:9-alpine'
          registryUrl 'https://index.docker.io/v1/'
        }
      }

      steps {
        sh 'yarn'
      }
    }

    stage('Linting') {
      when { not { branch 'master' } }

      agent {
        docker {
          image 'tommymccallig/codelint:0.0.1'
          registryUrl 'https://index.docker.io/v1/'
        }
      }

      environment {
        PRONTO_GITHUB_ACCESS_TOKEN = credentials('PRONTO_GITHUB_ACCESS_TOKEN')
        PRONTO_PULL_REQUEST_ID = "${env.CHANGE_ID}"
      }

      steps {
        sh 'pronto run -f github_status github_pr_review -c origin/master'
      }
    }
  }
}
