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
        sh 'mvn cobertura:cobertura test'
      }
    }
    stage('package') {
      parallel {
        stage('package') {
          steps {
            sh 'mvn package'
          }
        }
        stage('report') {
          steps {
            junit 'target/surefire-reports/*.xml'
          }
        }
        stage('coverage report') {
          steps {
            cobertura(coberturaReportFile: 'target/site/cobertura/coverage.xml')
          }
        }
      }
    }
    stage('deploy') {
      parallel {
        stage('deploy') {
          steps {
            sh 'make deploy-default'
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
      sh 'docker-compose run clean'

    }

    success {
      echo 'success!'

    }

    failure {
      echo 'failure!'

    }

  }
}