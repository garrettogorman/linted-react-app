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

    stage('Lint') {
      when { not { branch 'master' } }

      agent {
        docker {
          image 'tommymccallig/codelint:0.0.1'
          registryUrl 'https://index.docker.io/v1/'
        }
      }

      environment {
        PRONTO_TOKEN = credentials('PRONTO_GITHUB_ACCESS_TOKEN')
      }

      steps {
        echo 'Linting...'

        sh 'pronto run -f github_status github_pr_review -c origin/master'

        // script {

        //   docker.image('tommymccallig/codelint:0.0.1').inside(
        //     "-e PRONTO_GITHUB_ACCESS_TOKEN=${env.PRONTO_TOKEN} " +
        //     "-e PRONTO_PULL_REQUEST_ID=${env.CHANGE_ID} "
        //   ){
        //     sh 'pronto run -f github_status github_pr_review -c origin/master'
        //   }
        // }
      }
    }

    /*
    stage('Build Image'){
      steps {
        echo 'Building docker image...'

        script {
          def app = docker.build('jenkins-docker-sample')

          // If the database is still initialising, we wait
          ensureDatabase()

          def testConfig =
            '--network jenkins_test ' +
            '-e "DATABASE_URL=mysql2://root:my-secret-pw@jenkinsmysql/testdb"'

          app.inside(testConfig) {
            sh 'rake db:setup'
            sh 'rake db:migrate'
            sh 'rspec --format progress --format RspecJunitFormatter --out tmp/rspec.xml'
          }
        }
      }

      post {
        always {
          junit 'tmp/rspec.xml'
        }
      }
    }
    */
  }
}
