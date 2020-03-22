pipeline {
  agent any

  stages {
    stage('Build Image') {
      steps {        
        sh "docker build -t harbor01.infra.drie.it/pascal/jimpinit:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'drieit', passwordVariable: 'harborPassword', usernameVariable: 'harborUser')]) {
          sh "docker login -u ${env.harborUser} -p ${env.harborPassword} harbor01.infra.drie.it"
          sh "docker push harbor01.infra.drie.it/pascal/jimpinit:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Remove Unused docker image') {
      steps {
        sh "docker rmi harbor01.infra.drie.it/pascal/jimpinit:${env:BUILD_NUMBER}"
      }
    }
  }
}
