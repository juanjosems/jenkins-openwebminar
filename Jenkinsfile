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
        sh 'docker run --rm --name app -id -p 80:80 app:test'
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
          withDockerRegistry([ credentialsId: "docker", url: "" ]){
              sh 'docker tag app:test juanjosems/app:stable'
              sh 'docker push juanjosems/app:stable'
          }
      }
    }
  }
}