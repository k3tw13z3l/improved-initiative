pipeline {
  agent { dockerfile true }
  environment {
    registry = "harbor01.infra.drie.it/pascal"
    registryCredential = 'drieit'
  }
  stages {
    stage('Build') {
      steps {
        checkout scm
        def PkImage = docker.build registry + ("/jimpinit:${env.BUILD_ID}")
        docker.withRegistry ('', registryCredentials ) {
           PkImage.push()
           PkImage.push('latest')
        }
      }
    }
  }
}
