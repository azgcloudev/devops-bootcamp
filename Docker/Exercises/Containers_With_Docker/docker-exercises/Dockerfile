FROM openjdk:22-ea-24-jdk-oraclelinux8

EXPOSE 8080

RUN mkdir -p /home/app

WORKDIR /home/app

COPY ./build/libs/docker-exercises-project-1.0-SNAPSHOT.jar ./

CMD [ "java", "-jar", "docker-exercises-project-1.0-SNAPSHOT.jar" ]