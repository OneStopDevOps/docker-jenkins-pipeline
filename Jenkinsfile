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

    IMAGE_NAME = 'onestopdevops/project1-docker-jenkins-pipeline'

    COMPOSE_FILE = 'docker-compose.yml'

  }

  stages {

    stage('Clone docker-jenkins-pipeline') {

      steps {
        checkout scm
      }
    }

    stage('Build jar') {

      steps {
         echo "Building inventory-service..."

         dir('inventory-service') {
          sh 'pwd'
          sh 'mvn clean package'
         }
      }
    }

    stage('Build and run docker image') {
      
      steps {

        echo "Building inventory-service image."
        sh 'export BUILD_VERSION=${env.BUILD_TAG}&& docker-compose build'
        sh 'docker-compose up -d'
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