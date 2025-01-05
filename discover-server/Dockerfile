# Use Maven image to build the application
FROM maven:3.8.5-openjdk-11 AS build

# Add metadata labels
LABEL maintainer="Ryan Jay Flores <ryanjayf@gmail.com>"
LABEL version="1.0"
LABEL description="Spring Boot Eureka Discovery Service"
LABEL org.opencontainers.image.source="https://github.com/ryanjayf/io.discovery.git"

# Set the working directory inside the container
WORKDIR /app

# Copy Maven wrapper files and pom.xml first to leverage Docker layer caching
COPY pom.xml mvnw mvnw.cmd ./
COPY .mvn .mvn

# Download dependencies
RUN ./mvnw dependency:resolve

# Copy application source code
COPY src ./src

# Build the application
RUN ./mvnw package -DskipTests

# Use OpenJDK 11 for runtime
FROM openjdk:11-jdk-slim

# Set the working directory for the runtime container
WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose port 8761 for Eureka
EXPOSE 8761

# Set the entry point to run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]