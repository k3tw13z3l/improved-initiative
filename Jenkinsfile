pipeline {
  agent any
  environment {
    registry = "harbor01.infra.drie.it/improved_init" 
    registryCredential = 'drieit'
  }
  stages {
    stage('Build Image') {
      steps {        
        sh "docker build -t harbor01.infra.drie.it/improved_init/jimpinit:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'drieit', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "echo ${env.dockerHubUser}"
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword} harbor01.infra.drie.it"
          sh "docker push harbor01.infra.drie.it/improved_init/jimpinit:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi harbor01.infra.drie.it/improved_init/jimpinit:${env:BUILD_NUMBER}"
      }
    }
  }
}
