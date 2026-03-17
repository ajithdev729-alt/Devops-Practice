# STAGE 1: The Build Stage
FROM maven:3.9-amazoncorretto-17 AS builder

WORKDIR /app

# Enable layer caching by copying pom.xml first
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build
COPY src ./src
RUN mvn clean package -DskipTests

# STAGE 2: The Run Stage 
FROM amazoncorretto:17-alpine

# Set up non-root user
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

WORKDIR /app

# Copy the built jar from the builder stage
COPY --from=builder /app/target/*.jar app.jar

ENTRYPOINT ["java", "-jar", "app.jar"]
