pipeline {
  agent any
  stages {
    stage('checkout project') {
      steps {
        checkout scm
      }
    }
    stage('test') {
      steps {
        sh '''docker-compose run test
'''
      }
    }
    stage('report') {
      parallel {
        stage('report') {
          steps {
            cobertura(coberturaReportFile: ' target/site/cobertura/coverage.xml')
          }
        }
        stage('junit report') {
          steps {
            junit 'target/surefire-reports/*.xml'
          }
        }
      }
    }
    stage('package') {
      steps {
        sh 'docker-compose run package'
      }
    }
    stage('deploy') {
      parallel {
        stage('deploy') {
          steps {
            sh '''make build-docker-prod-image
make deploy-production-ssh
'''
          }
        }
        stage('archive') {
          steps {
            archiveArtifacts 'target/spring-boot-sample-data-rest-0.1.0.jar'
          }
        }
      }
    }
  }
  post {
    always {
      echo 'I will always say Hello again!'

    }

    success {
      echo 'success!'

    }

    failure {
      echo 'failure!'

    }

  }
}