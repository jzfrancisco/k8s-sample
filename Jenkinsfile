pipeline {

  environment {
    registry = "172.17.0.11:5000/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/jzfrancisco/playjenkins.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          sh "docker build -t test:${BUILD_NUMBER} ."
          dockerImage = "test:${BUILD_NUMBER}"
        }
      }
    }

    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml")
        }
      }
    }

  }
}
