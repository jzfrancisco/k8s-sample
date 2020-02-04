pipeline {

  environment {
    registry = "localhost:5000/myweb"
  }

  agent {
    label 'docker'
  }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/jzfrancisco/playjenkins.git'
      }
    }

    stage('Build image') {
      agent {
        docker {
          steps {
            script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
          }
        }
      }
    }

    stage('Push Image') {
      agent {
        docker {
          steps {
            script {
              docker.withRegistry( "" ) {
                dockerImage.push()
              }
            }
          }
        }
      }
    }

    stage('Deploy App') {
      agent {
        docker {
          steps {
            script {
              kubernetesDeploy(configs: "myweb.yaml")
            }
          }
        }
      }
    }

  }

}
