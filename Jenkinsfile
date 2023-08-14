pipeline {

  environment {
    registry = "simon9799/flask"
    registry_mysql = "simon9799/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/simon9799/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('docker login') {
      steps {
        withCredentials([usernameColonPassword(credentialsId: '718c49db-45a2-4fef-8b11-c84f50a2fe4a', variable: 'https://hub.docker.com/')])
    
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "simon9799/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "simon9799/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }
}
