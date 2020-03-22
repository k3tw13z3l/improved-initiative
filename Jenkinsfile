pipeline {
  agent { dockerfile true }
  environment {
    registry = "harbor01.infra.drie.it/improved_init" 
    registryCredential = 'drieit'
  }
  stages {
    stage('Build Image') {
      steps {
        checkout scm
        
        sh 'docker build -t $registry + "/jimpinit:${env.BUILD_NUMBER} ."'
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'drieit', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push $registry + "/jimpinit:${env.BUILD_NUMBER}"'
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry+jimpinit:${env:BUILD_NUMBER}"
      }
    }
  }
}
