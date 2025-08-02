# This Dockerfile builds the Java application using Maven and runs it in a lightweight JDK environment
# The application is built in a multi-stage build process to keep the final image small

# Use Maven with JDK 17 as the base image for building the application
FROM maven:3.8-jdk-17 AS builder

# Set the working directory for the build stage
WORKDIR /build

# Copy the Maven project file to the build directory
COPY pom.xml .

# Copy the source code into the build directory
COPY src ./src

# Build the application using Maven, skipping tests to speed up the build process
RUN mvn package -DskipTests

# Use a lightweight JDK image for the final application
FROM eclipse-temurin:17-jdk-alpine

# Expose the application port
EXPOSE 8080 

# Set the application home directory
ENV APP_HOME /usr/src/app

# Copy the built application from the builder stage
WORKDIR $APP_HOME

# Copy the JAR file to the application home directory
# and Set the command to run the application
COPY --from=builder /build/target/app.jar ./app.jar

# Use the JAR file as the entry point for the container
CMD ["java", "-jar", "app.jar"]
