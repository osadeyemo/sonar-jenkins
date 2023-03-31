pipeline {
  environment {
    imagename = "fdjapi10/sonar"
    registryCredential = 'dockerhub-creds'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/djapif10/sonar-jenkins.git', branch: 'main'])
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
    stage('Run Application') {
      steps {
        sh "docker run -d --name myapp -p 9000:9000 $imagename:$BUILD_NUMBER"
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
     }
    }
  }
}
