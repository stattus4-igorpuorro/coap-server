FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

COPY pom.xml .

RUN mvn dependency:resolve

COPY src ./src

RUN mvn clean package -DskipTests

FROM openjdk:21-jdk-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends net-tools && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=builder /app/target/coap-server-0.0.1-SNAPSHOT-jar-with-dependencies.jar app.jar

EXPOSE 5683/udp 5684/udp

ENTRYPOINT ["java", "-jar", "app.jar"]
