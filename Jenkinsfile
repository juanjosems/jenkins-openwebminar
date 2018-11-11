pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t app:test .'
      }
    }
    stage('Test') {
      steps {
        echo 'Test'
        sh 'docker run -itd -p 80:80 --rm app'
        sh '/bin/nc -vz localhost 22'
        sh '/bin/nc -vz localhost 80'
      }
      post{
        always {
            sh 'docker container stop app'
        }
      }
    }
    stage('Push registry') {
      steps {
        sh 'docker tag app:test juanjosems/app:stable'
        sh 'docker push juanjosems/app:stable'
      }
    }
  }
}