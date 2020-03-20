node {
    environment {
       registry = "harbor01.infra.drie.it/pascal"
       registryCredential = 'drieit'
    }
    stage('Build') {
       Steps {
          checkout scm
          def PkImage = docker.build registry + ("/jimpinit:${env.BUILD_ID}")
          docker.withRegistry ('', registryCredentials ) {
             PkImage.push()
             PkImage.push('latest')
          }
       }
    }
}
