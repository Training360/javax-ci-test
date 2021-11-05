# CI/CD implementálása Maven/Gradle, Jenkins és Docker eszközökkel tanfolyam

https://groovy-lang.org/style-guide.html

```shell
gradlew build
```

A `build/libs` kvt-ban létrejön egy `employees-0.0.1-SNAPSHOT.jar`

Ezt lehet futtatni:

```shell
java -jar employees-0.0.1-SNAPSHOT.jar
```

Kipróbálható a http://localhost:8080/swagger-ui.html címen.

```shell
docker run -d -e MYSQL_DATABASE=employees -e MYSQL_USER=employees -e MYSQL_PASSWORD=employees   -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 --name employees-mariadb mariadb
```

```shell
docker exec -it employees-mariadb mysql employees
```

Opcionális:

```sql
create table employees (id bigint not null auto_increment, emp_name varchar(255), primary key (id));
insert into employees(emp_name) values ("John Doe");
select * from employees;
drop table employees;
```

`build.gradle` állományba felveszed a `systemProperty`-ket (3 sor), `gradlew build`

## Docker image futtatása

Docker image: https://hub.docker.com/repository/docker/training360/employees-evo

```shell
docker run -p 8085:8080 --name tr360-employee training360/employees-evo
```

Utána a http://127.0.0.1:8085 címen bejön a `Hello Training!!!`.

## Image készítése

```shell
gradlew clean assemble

docker build -t employees .

docker run -p 8080:8080 --name my-employees employees
```

## Két kommunikáló container

```shell
docker network create employees-net

docker run -d --network employees-net -e MYSQL_DATABASE=employees -e MYSQL_USER=employees -e MYSQL_PASSWORD=employees -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3307:3306 --name emp-app-mariadb mariadb

docker network inspect employees-net

docker run -d --network employees-net -p 8081:8080 --name emp-app -e SPRING_DATASOURCE_URL=jdbc:mariadb://emp-app-mariadb/employees -e SPRING_DATASOURCE_USERNAME=employees -e SPRING_DATASOURCE_PASSWORD=employees employees

docker exec -it emp-app-mariadb  mysql employees
```

## Docker futtás Gradle-ből

```shell
gradlew dockerRun
```

## Layered

```shell
docker build -t employees --file Dockerfile.layered .
```

## Build in docker

```shell
docker build -t employees --file Dockerfile.localebuild .
```

## E2E

```shell
docker run  --entrypoint "" -v "/C/training/javax-ci-2021-05-03/employees/integrationtest/postman:/etc/newman"  postman/newman:5-ubuntu newman run --environment=integrationtest.postman_environment.json employees.postman_collection.json
```

## E2E Docker Compose

```shell
docker-compose up --abort-on-container-exit
```

## Jenkins

```shell
docker build -t employees-jenkins --file Dockerfile.jenkins .
docker network create jenkins
docker run --detach --network jenkins --volume jenkins-data:/var/jenkins_home --volume /var/run/docker.sock:/var/run/docker.sock --publish 8090:8080 --name employees-jenkins  employees-jenkins
```

Böngésző: `http://localhost:8090/`

Generált jelszó (`docker logs -f employees-jenkins` vagy `docker exec -it employees-jenkins cat /var/jenkins_home/secrets/initialAdminPassword`)

Pluginek feltelepítése - Custom/Next

Admin felhasználó tulajdonságait: `admin`, e-mail: `admin@admin.admin`

## Saját build

https://github.com/Training360/employees-20210505

```shell
docker exec --user root -it employees-jenkins chmod 777 /var/run/docker.sock
```

## SonarQube

```shell
docker run --name employees-sonarqube --detach --network jenkins --publish 9000:9000 sonarqube:lts
```