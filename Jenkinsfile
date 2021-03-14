/*
  A simple Jenkins pipeline build instruction that does the following:
    - build the inventory-service Java application
    - create a docker image 
    - push the docker image to DockerHub
    - run the image as a container
*/
pipeline {

  agent any

  stages {

    stage('Cloning docker-jenkins-pipeline') {

      steps {
        git([url: 'https://github.com/OneStopDevOps/docker-jenkins-pipeline.git', branch: 'master', 
           credentialsId: 'onestopdevops-github-user-token'])
      }
    }

    stage('Building jar') {

      steps {
         echo "Building inventory-service..."

         sh 'mvn clean package'
      }
    }
  }

  post {

    success {
      echo "Build completed."
    }

    failure {
      echo "Build failed."
    }
  }

}