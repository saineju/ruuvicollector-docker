FROM maven:3.5.3-jdk-8-alpine AS builder
RUN apk update
RUN apk add bluez bluez-deprecated git
WORKDIR /tmp
RUN git clone https://github.com/Scrin/RuuviCollector
WORKDIR /tmp/RuuviCollector
RUN mvn clean package

FROM openjdk:jre-alpine

RUN mkdir /app
RUN apk update && apk add bluez bluez-deprecated
COPY --from=builder /tmp/RuuviCollector/target/ruuvi-collector-0.2.jar /app/ruuvi-collector-0.2.jar
WORKDIR /app
CMD ["java", "-jar", "ruuvi-collector-0.2.jar"]
