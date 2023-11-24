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

```Docker
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

### Build the image locally and push to this repository
Build the image and tag the image to upload it to the repository.

```bash
docker build -t app:1.0 .

docker tag app:1.0 nexusserver:8083/app:1.0

docker login nexusserver:8083

docker push nexusserver:8083/app:1.0
```

## Exercise 6

###

```yaml
version: '3'
services:
  mysqldb:
    image: mysql
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MY_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes:
      - mysql-data:/var/lib/mysql
    container_name: mysql

  phpmyadmin:
    image: phpmyadmin:5.2-apache
    ports:
     - 8181:80
    environment:
      - PMA_ARBITRARY=1

  java-app:
    image: app:1.2
    ports:
      - 8080:8080
    environment:
      - DB_NAME=${DB_NAME}
      - DB_SERVER=${DB_SERVER}
      - DB_PWD=${DB_PWD}
      - DB_USER=${DB_USER}
    depends_on:
      - mysqldb

volumes:
  mysql-data:
```

## Exercise 7

### Set insecure docker repository on server, because Nexus uses http
Added the Nexus repo to the deamon file in docker to access via http. 

*cat /etc/docker/daemon.json*
```json
{
	"insecure-registries": ["4.151.179.169:8083"]
}
```

### Run docker login on the server to be allowed to pull the image
```bash
docker login 4.151.179.169:8083
```

### Your application index.html has a hardcoded localhost as a HOST to send requests to the backend. You need to fix that and set the server IP address instead, because the server is going to be the host when you deploy the application on a remote server. (Don't forget to rebuild and push the image and if needed adjust the docker-compose file)

Updated the variable in the index.html file.

### Copy docker-compose.yaml to the server
```bash
scp -i /Users/aldairzamora/Documents/aldairdevops.pem -r docker-compose.yaml azureuser@4.151.146.65:/home/azureuser/application/
```

### Set the needed environment variables for all containers in docker-compose
```bash
## MySQL variables
export MYSQL_ROOT_PASSWORD=Administrator123
export MYSQL_DATABASE=Test
export MYSQL_USER=java
export MYSQL_PASSWORD=Administrator123

## Java application variables
export DB_NAME=Test
export DB_SERVER=mysqldb
export DB_PWD=Administrator123
export DB_USER=java
```

### Run docker-compose to start all 3 containers
```bash
docker compose -f docker-compose.yaml up -d
```

