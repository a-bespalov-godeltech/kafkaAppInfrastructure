FROM alpine:latest AS sources
RUN apk add --no-cache git
RUN rm -rf /home/kafkaProducerApp \
      mkdir /home/kafkaProducerApp
RUN git clone --branch feature/client https://github.com/a-bespalov-godeltech/kafkaProducerApp.git home/kafkaProducerApp


FROM gradle:6.9.0-jdk8 AS build
COPY --chown=gradle:gradle --from=sources /home/kafkaProducerApp /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle build --no-daemon


FROM openjdk:8-jre-slim
WORKDIR /home/kafkaProducerApp/
EXPOSE 8080
RUN mkdir /app
COPY --from=build /home/gradle/src/build/libs/*.jar /app/producer.jar

ENTRYPOINT ["java", "-XX:+UnlockExperimentalVMOptions", "-jar","/app/producer.jar"]
