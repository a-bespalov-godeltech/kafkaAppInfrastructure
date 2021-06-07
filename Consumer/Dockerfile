FROM alpine:latest AS sources
RUN apk add --no-cache git
RUN rm -rf /home/kafkaConsumerApp \
      mkdir /home/kafkaConsumerApp
RUN git clone --branch feature/client https://github.com/a-bespalov-godeltech/kafkaConsumerApp.git home/kafkaConsumerApp


FROM gradle:6.9.0-jdk8 AS build
COPY --chown=gradle:gradle --from=sources /home/kafkaConsumerApp /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle build --no-daemon


FROM openjdk:8-jre-slim
WORKDIR /home/kafkaConsumerApp/
EXPOSE 8081
RUN mkdir /app
COPY --from=build /home/gradle/src/build/libs/*.jar /app/consumer.jar

ENTRYPOINT ["java", "-XX:+UnlockExperimentalVMOptions", "-jar","/app/consumer.jar"]
