pipeline {
  agent { dockerfile true }
  environment {
    registry = "harbor01.infra.drie.it/improved_init" 
    registryCredential = 'drieit'
  }
    stage('Docker Build') {
      steps {
        sh "docker build -t kmlaydin/podinfo:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push kmlaydin/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Image') {
      steps {
        sh "docker rmi kmlaydin/podinfo:${env.BUILD_NUMBER}"
      }
    }  
  
  
  stages {
    stage('Build Image') {
      steps {
        checkout scm
        script {
          def dockerImage = docker.build $registry + "/jimpinit:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'drieit', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push $registry +  "/jimpinit:${env.BUILD_NUMBER}"   
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry+jimpinit:${env:BUILD_NUMBER}"
      }
    }
  }
}
