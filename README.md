# CalTech DevOps Project 1 - Docker Jenkins Pipeline

## Project Description:

This is a simple project that demonstrates how to maintain a continuous integration and delivery processes by using Jenkins pipeline and docker.
The Jenkins pipeline will do the following tasks:

 - perform code checkout from Git repo
 - build the sample Java SpringBoot application using maven
 - build the docker image
 - run it as a container
 - push the docker image to DockerHub repo
 - remove the docker image
 - finally send out email notification on the build status.
