# Docker exercises

## Exercise 1

### MySQL installation

To install and ru mysql did:

```bash
docker run -d -p 3306:3306 --name mysql-db -e MYSQL_ROOT_PASSWORD=Administrator123 -e MYSQL_DATABASE=Test -e MYSQL_USER=admin -e MYSQL_PASSWORD=Administrator123 mysql:8.2.0-oracle
```

### Gradle build

To build the application, ran:

```bash
gradle build
```

### Env variables

Ran the following in the local terminal:

```bash
export DB_NAME=Test
export DB_SERVER=localhost
export DB_PWD=Administrator123
export DB_USER=admin
```

### Run the Java application

```bash
cd build/libs
java -jar filename.jar
```

## Exercise 2

Will be installing and using phpmyadmin to access MySQL DB with a GUI interface.

### Download image

```bash
docker pull phpmyadmin:5.2-apache
```

### Run phpmyadmin

```bash
docker run --name phpmyadmin -p 8181:80 -d --link mysql-db:db phpmyadmin:5.2-apache
```

## Exercise 3

### Use docker compose for MySQL and Phpmyadmin

Deploy's MySQL database and Phpmyadmin from a docker compose file.

```yaml
version: '3'
services:
  mysql:
    image: mysql
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=Administrator123
      - MYSQL_DATABASE=Test
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=Administrator123
    volumes:
      - mysql-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:5.2-apache
    ports:
     - 8181:80
    environment:
      - PMA_ARBITRARY=1
volumes:
  mysql-data:
```

## Exercise 4

### Dockerize the java application

```
FROM openjdk:22-ea-24-jdk-oraclelinux8

RUN mkdir -p /home/app

COPY ./build/libs/docker-exercises-project-1.0-SNAPSHOT.jar /home/app

WORKDIR /home/app

EXPOSE 8080

CMD [ "java", "-jar", "docker-exercises-project-1.0-SNAPSHOT.jar" ]
```

## Exercise 5 (Build and push Java Application Docker Image)

### Create the hosted repository in nexus
From the nexus GUI created a docker hosted repository.
Enabled the following:
- Allow anonymous docker pull.
- Enabled HTTP port 8083.
- On realms tab:
  - Activated *Docker Bearer Token Realm*

On docker desktop added the repo to access via HTTP.