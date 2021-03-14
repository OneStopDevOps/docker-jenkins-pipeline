# Use a slim version of openjdk 8 to reduce memory footprint and fast startup
FROM openjdk:8-jre-slim

LABEL maintainer="jane.doe@yahoo.com"

# Rename the jar 
ADD ["target/inventory-service-0.0.1-SNAPSHOT.jar", "app.jar"]

# Expose port 
EXPOSE 8081

# Update the metadata of the file
RUN /bin/sh -c 'touch app.jar'

ENTRYPOINT ["sh","-c","java -jar app.jar"]