# Stage 1: Build the .jar file
FROM maven:3.8.6-openjdk-11 as build

# Set the working directory in the container
WORKDIR /app

# Copy the pom.xml and install dependencies first to take advantage of Docker's cache
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the rest of the source code
COPY src /app/src

# Build the .jar file
RUN mvn clean package -DskipTests

# Stage 2: Create a smaller image for running the application
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /app

# Copy the jar file from the previous build stage
COPY --from=build /app/target/your-app.jar /app/your-app.jar

# Run the application
CMD ["java", "-jar", "/app/your-app.jar"]
