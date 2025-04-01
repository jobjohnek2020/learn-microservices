FROM openjdk:17-ea-slim AS build
WORKDIR /app
ARG JAR_FILE=target/configserver-application.jar
COPY ${JAR_FILE} configserver-application.jar
RUN java -Djarmode=layertools -jar configserver-application.jar extract

FROM openjdk:17-ea-slim
WORKDIR /app
COPY --from=build /app/dependencies/ ./
COPY --from=build /app/spring-boot-loader/ ./
COPY --from=build /app/snapshot-dependencies/ ./
COPY --from=build /app/application/ ./

ENTRYPOINT ["java","org.springframework.boot.loader.launch.JarLauncher"]