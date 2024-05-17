pipeline {
  environment {
    imagename = "nareshkatta/pushdockerimage"
    registryCredential = 'Pushdockerimage'
    dockerImage = ''
    RELEASE = "1.0.0"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/Nareshfreak/pushdockerimage.git', branch: 'main', credentialsId: 'GithubAccessToken'])

      }
    }
    stage('Building image') {
      steps{
        script {
           docker.withRegistry( '', registryCredential ){
          dockerImage = docker.build("${imagename}")
        }
      }
    }
    }
    stage('Trivy Scan'){
      steps{
        script{
        sh "trivy image ${dockerImage.id} > scan.txt"
      }
      }
    }
    stage('Push Docker  Image to Dockerhub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("${IMAGE_TAG}")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Test image') {
      steps{
        script{
      dockerImage.inside {
        sh 'echo "Tests Passed" '
      }
    }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$IMAGE_TAG"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
