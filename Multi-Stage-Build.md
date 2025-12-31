# Multi Stage Build

- A multi-stage build in Docker refers to a feature that allows you to define multiple build stages within a single Dockerfile.
  
- Each stage represents a phase in the build process, and the output of one stage can be used as the input for the next stage.
  
- This feature is designed to help optimize the size of the final Docker image by discarding unnecessary artifacts and dependencies from intermediate stages.

- **real advantage of multistage docker build and distro less images can be understood with a drastic decrease in the Image size.**


Below is the example of the multi-stage build.

```
# Stage 1: Build Stage
FROM maven:3.8.1-openjdk-11 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src src
RUN mvn package

# Stage 2: Run Stage
FROM tomcat:9.0-jdk11-openjdk-slim
WORKDIR /usr/local/tomcat/webapps
COPY --from=builder /app/target/maven-web-app.war .
CMD ["catalina.sh", "run"]
```

•	Stage 1 (Build Stage):

      •	Uses a Maven image to build the Java application.
      
      •	Copies the pom.xml file, downloads dependencies, and copies the source code.
      
      •	Runs the Maven package phase to build the JAR file.
    
•	Stage 2 (Run Stage):

      •	Uses a smaller OpenJDK image suitable for running the application.
      
      •	Copies the JAR file from the build stage into the final image.
      
      •	Sets the default command to run the Java application.
      

```Use case:``` They are also beneficial for reducing the size of final images in scenarios where you need specific build tools or dependencies only during the build phase but not at runtime.
