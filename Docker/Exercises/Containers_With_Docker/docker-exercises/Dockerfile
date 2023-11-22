FROM openjdk:22-ea-24-jdk-oraclelinux8

RUN mkdir -p /home/app

COPY ./build/libs/docker-exercises-project-1.0-SNAPSHOT.jar /home/app

WORKDIR /home/app

EXPOSE 8080

CMD [ "java", "-jar", "docker-exercises-project-1.0-SNAPSHOT.jar" ]