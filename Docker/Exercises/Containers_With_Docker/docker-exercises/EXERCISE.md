# Exercise 1

## MySQL installation
To install and ru mysql did:
```bash
docker run -d -p 3306:3306 --name mysql-db -e MYSQL_ROOT_PASSWORD=Administrator123 -e MYSQL_DATABASE=Test mysql:8.2.0-oracle
```

## Gradle build
To build the application, ran:
```bash
gradle build
```

## Env variables
Ran the following in the local terminal:
```bash
export DB_NAME=Test
export DB_Server=localhost
export DB_PWD=Administrator123
export DB_USER=root
```
## Run the Java application
```bash
cd build/libs
java -jar filename.jar
```