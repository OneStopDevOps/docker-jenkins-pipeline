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
    DOCKER_CREDENTIALS  = 'onestopdevops-dockerhub-user-token'
  }

  stages {

    stage('Stage 1 - Clone docker-jenkins-pipeline') {

      steps {

        sendStartNotification()

        echo "Checking out from github repo..."
        checkout scm
      }
    }

    stage('Stage 2 - Build jar') {

      steps {
         echo "Building inventory-service..."

         dir('inventory-service') {
          sh 'pwd'
          sh 'mvn clean package'
         }
      }
    }

    stage('Stage 3 - Build and run docker image') {
      
      steps {

        echo "Building inventory-service image..."
        sh "export BUILD_VERSION=${BUILD_TAG}&& docker-compose build"
        sh "export BUILD_VERSION=${BUILD_TAG}&& docker-compose up -d"
      }
    }

    stage('Stage 4 - Pushing docker image to DockerHub') {
      
      steps {

        script {
          echo "Uploading image to DockerHub..."
          docker.withRegistry('', "${DOCKER_CREDENTIALS}") {

            // pushing image 
            sh "export BUILD_VERSION=${BUILD_TAG}&& docker-compose -f ${env.COMPOSE_FILE} push"
            echo "Image pushed."
          }
        }
      }
    }

  }

  post {

    success {
      echo "Build completed."
      sendSuccessNotification()
    }

    failure {
      echo "Build failed."
      sendFailNotification()
    }
    
    always {
        echo "Shutting down container..."
        sh "export BUILD_VERSION=${BUILD_TAG}&& docker-compose down --remove-orphans"
        echo "Removing image..."
        sh "docker rmi ${env.IMAGE_NAME}:${BUILD_TAG}"
    }
  }

}

/*
   Send out start build event notification thru email
*/
def sendStartNotification() {

  emailext( 
    subject: "STARTED Job: '${env.JOB_NAME} [${env.JOB_NUMBER}]'",
    body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
    recipientProviders: [[$class: 'DevelopersRecipientProvider']] 
  )
}

/*
  Send out fail build event notification thru email
*/
def sendFailNotification() {

  emailext (
    subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
    recipientProviders: [[$class: 'DevelopersRecipientProvider']]
  )
}

/*
  Send out success build event notification thru email
*/
def sendSuccessNotification() {

  emailext (
    subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    to: 'jastaoathk'
    body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
      <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
    recipientProviders: [[$class: 'DevelopersRecipientProvider']]
  )
} 