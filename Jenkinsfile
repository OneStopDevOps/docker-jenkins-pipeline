/*
  A simple Jenkins pipeline build instruction that does the following:
    - build the inventory-service Java application
    - create a docker image 
    - push the docker image to DockerHub
    - run the image as a container
*/
pipeline {

  agent any

  environment {

    // Reserved for docker environment variable
  }

  stages {

    stage('Building jar') {

      when {
        branch 'origin/master'
      }

      steps {
         echo "Building inventory-service..."

         sh 'mvnw clean package'
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

}