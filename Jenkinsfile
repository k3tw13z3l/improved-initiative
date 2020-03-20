pipeline {
  agent { dockerfile true }
  environment {
    registry = "harbor01.infra.drie.it/pascal"
    registryCredential = 'drieit'
  }
  stages {
    stage('Build Image') {
      steps {
        checkout scm
        def dockerImage = docker.build registry + "/jimpinit:$BUILD_NUMBER"
        docker.withRegistry ('', registryCredentials ) {
           dockerImage.push()
           dockerImage.push('latest')
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
