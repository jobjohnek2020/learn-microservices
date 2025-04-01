FROM maven:3.8.4-openjdk-11-slim AS build
# Set the working directory in the container
WORKDIR /app
# Copy the pom.xml and the project files to the container
COPY pom.xml .
COPY src ./src
# Build the application using Maven
RUN mvn clean install

FROM openjdk:17-ea-slim
WORKDIR /app
ARG JAR_FILE=/app/target/*.jar
COPY $JAR_FILE configserver-application.jar
RUN java -Djarmode=layertools -jar configserver-application.jar extract

FROM openjdk:17-ea-slim
WORKDIR /app
COPY --from=build /app/dependencies/ ./
COPY --from=build /app/spring-boot-loader/ ./
COPY --from=build /app/snapshot-dependencies/ ./
COPY --from=build /app/application/ ./

ENTRYPOINT ["java","org.springframework.boot.loader.launch.JarLauncher"]