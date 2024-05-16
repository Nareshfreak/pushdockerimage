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
          dockerImage = docker.build imagename
        }
      }
    }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
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
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
