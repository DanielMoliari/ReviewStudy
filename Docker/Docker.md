

<iframe width="702" height="395" src="https://www.youtube.com/embed/Kzcz-EVKBEQ" title="Docker em 22 minutos - teoria e prática (Rápido!)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
docker build -t mysql-image -f api/directory/Dockerfile .

docker run -d --rm --name mysql-container mysql-image

docker ps

docker exec -it mysql-container /bin/bash

docker stop mysql-container

docker run -d -v $(pwd)/api/db/data:/var/lib/mysql --rm --name mysql-container mysql-image

docker run -d -v $(pwd)/api:/home/node/app -p 9001:9001 --link mysql-container --rm --name node-container node-image


<iframe width="702" height="395" src="https://www.youtube.com/embed/01MR38eDXz8" title="Introdução ao Docker para iniciantes | Docker Tutorial #docker" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
docker run --name mysql_container -e MYSQL_ROOT_PASSWORD=root mysql:5.7

docker ps

docker stop d3

docker ps -a

docker start d3

docker rm d3

docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:5.7

npx create-react-app .

Dockerfile  -> 

{
FROM node:16-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "npm", "start" ]
}

docker build -t meu_app .

docker run --name react_app -p 3000:3000 meu_app

<iframe width="702" height="395" src="https://www.youtube.com/embed/DdoncfOdru8" title="APRENDA DOCKER DO ZERO | TUTORIAL COMPLETO COM DEPLOY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Registry (Docker Hub, Azure)

Images

Container (Image-instance running)

docker run hello-world

docker run -it ubuntu bash

Dockerfile ->

{
FROM maven:3.8.4-jdk-8 AS build

COPY src /app/src
COPY pom.xml /app

WORKDIR /app

RUN mvn clean install

FROM openjdk:8-jre-alpine

COPY --from=build /app/target/spring-boot-2-hello-world-1.0.2-SNAPSHOT.jar

WORKDIR /app

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
}

docker build -t spring-boot-hello-world:1.0 .

docker run spring-boot-hello-world:1.0

docker run -p 8080:8080 spring-boot-hello-world:1.0



<iframe width="702" height="395" src="https://www.youtube.com/embed/K1V0FVDdluY" title="Aprenda Docker do Zero, tutorial passo a passo." frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>




<iframe width="702" height="395" src="https://www.youtube.com/embed/MeFyp4VnNx0" title="TUDO O QUE VOCÊ PRECISA SABER SOBRE DOCKER EM 2022" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

VM x Containers

VM ->

{
VM, VM, VM, VM
Hypervisor
SO
HW
} -> {
APP
SO
}

Container ->

{
C, C, C, C
Docker
SO
HW
} -> {
APP
}

Images divididas em camadas

Namespaces e Cgroups

Namespaces -> Isolar recursos, usuarios, isolar PID, network
Cgroups -> Isolamento CPU e Memória

docker run -d -p 80:80 docker/getting-started

docker container ls

docker logs -f 06d

docker rm
docker rmi

docker container exec -it 13efa7726f69 /bin/sh

docker image ls

docker image inspect getting-started:latest

docker container inspect 2034fd

docker build -t getting-started:2.0

docker image tag getting-started:latest getting-started:1.0

docker image tag getting-started:latest linuxtips/getting-started:latest

docker push linuxtips/getting-started:latest

docker login -u linuxtips

docker push linuxtips/getting-started:latest

docker volume create todo-db

docker volume ls

docker volume inspect todo-db

docker run -d -p 3000:3000 -v todo-db:/etc/todos/ linuxtips/getting-started:latest

docker network create todo-app

docker network ls

docker network create mysql-todo-db && docker run -d -v mysql-todo-db:/var/lib/mysql --network todo-app --network-alias mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:5.7

docker run -it --network todo-app nicolaka/netshoot

docker run -dp 3000:3000 \\
-w /app -v "$(pwd):/app" \\
--network todo-app \\
-e MYSQL_HOST=mysql \\
-e MYSQL_USER=root \\
-e MYSQL_PASSWORD=secret \\
-e MYSQL_DB=todos \\
node:12-alpine \\
sh -c "yarn install && yarn run dev"