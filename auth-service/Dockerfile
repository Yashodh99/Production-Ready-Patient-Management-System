FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

# Cache deps first
COPY pom.xml .
RUN mvn -B -DskipTests dependency:go-offline

# Then the sources
COPY src ./src

# Build without running tests (avoids contextLoads failures in builder)
RUN mvn -B clean package -DskipTests

FROM openjdk:21-jdk AS runner
WORKDIR /app

COPY --from=builder /app/target/auth-service-0.0.1-SNAPSHOT.jar ./app.jar

EXPOSE 4005
ENTRYPOINT ["java","-jar","/app/app.jar"]
