# This Dockerfile builds the Java application using Maven and runs it in a lightweight JDK environment
# The application is built in a multi-stage build process to keep the final image small

FROM maven:3.8-jdk-17 AS builder
# Use Maven with JDK 17 as the base image for building the application

WORKDIR /build
# Set the working directory for the build stage

COPY pom.xml .
# Copy the Maven project file to the build directory

COPY src ./src
# Copy the source code into the build directory

RUN mvn package -DskipTests
# Build the application using Maven, skipping tests to speed up the build process

FROM eclipse-temurin:17-jdk-alpine
# Use a lightweight JDK image for the final application

EXPOSE 8080 
# Expose the application port

ENV APP_HOME /usr/src/app
# Set the application home directory

WORKDIR $APP_HOME
# Copy the built application from the builder stage

COPY --from=builder /build/target/app.jar ./app.jar
# Copy the JAR file to the application home directory
# Set the command to run the application

CMD ["java", "-jar", "app.jar"]
# Use the JAR file as the entry point for the container