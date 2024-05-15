pipeline {
  environment {
    imagename = "nareshkatta/pushdockerimage"
    registryCredential = 'Pushdockerimage'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'hhttps://github.com/Nareshfreak/pushdockerimage.git', branch: 'master', credentialsId: 'GithubAccessToken'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
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
      dockerImage.inside {
        sh 'echo "Tests Passed" '
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
