FROM amazoncorretto:25

ARG JAR=target/*.jar

COPY $JAR /opt/app.jar

ENTRYPOINT ["java", "-jar", "/opt/app.jar"]
