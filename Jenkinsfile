pipeline {
   agent any
   environment {
       registry = "harbor01.infra.drie.it"
   }
   stages {
       stage('Build') {
           agent {
               docker {
                   image 'node:carbon'
               }
           }
           steps {
               // Create our project directory.
               sh 'cd /tmp/src'
               sh 'mkdir -p /tmp/src/impinit'
               // Copy all files in our Jenkins workspace to our project directory.
               sh 'cp -r ${WORKSPACE}/* /tmp/src/impinit'
               // Build the app.
               sh 'go build'
           }
       }
       stage('Test') {
       }
       stage('Publish') {
           environment {
               registryCredential = 'drieit'
           }
           steps{
               script {
                   def appimage = docker.build registry + ":$BUILD_NUMBER"
                   docker.withRegistry( '', registryCredential ) {
                       appimage.push()
                       appimage.push('latest')
                   }
               }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "ansible-playbook  playbook.yml --extra-vars \"image_id=${image_id}\""
               }
           }
       }
   }
}
