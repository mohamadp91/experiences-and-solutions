## docker push
1) login dockerhub.io
   

2) create a repository
  
 
3) login in  with `docker login -u username -p password --email emailAddress`


3) `docker tag imageId dockerId/repositoryName:tagname`
   

4) `docker push dockerId/repositoryName:tagname`

## A simple Dockerfile for build production in SpringBoot project
```
FROM maven:3.6.3-jdk-11-openj9 as app
COPY . ./app
WORKDIR /app
RUN mvn package -DskipTests
ENTRYPOINT [ "java","-jar","/app/target/[projectname]0.0.1-SNAPSHOT.jar" ]
```

_**maven:3.6.3-jdk-11-openj9**_ image includes a _**JDK**_ and _**MAVEN**_ platform to run java application and run _MVN_ commands.
_mvn package _ take the compiled code and package it in its distributable format , such as a JAR.
_**-DskipTests**_ is used to skip tests to create a jar file.

### A simple docker-compose.yml to connect Dockerfile above and postgresql container
```
version: "3.7"
services:
  app:
    depends_on:
      - postgre
    build:
      context: .
      target: app
    ports:
      - 8081:8081
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgre:5432/user
      - SPRING_JPA_HIBERNATE_DDL_AUTO=update
  postgre:
    container_name: postgre
    restart: always
    image: postgres:alpine
    expose:
      - 5432
    ports:
      - 5432:5432
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
      - POSTGRES_DB=user
```

The most important point is environments. Those environments are used instead of _**application.properties**_ file.

## A simple Dockerfile to create an image for react project and run it with nginx and viewing The Interactive Cypress Test Runner In Docker
```
FROM node:14-alpine as builder
RUN apk add --no-cache git
WORKDIR /usr/src/app
ENV PATH /app/node_modules/.bin:$PATH
COPY . .
RUN yarn install && yarn build && rm -rf src

FROM cypress/included:6.2.0 as test-runner
WORKDIR /app
COPY --from=builder /usr/src/app /app

FROM nginx:alpine as nginx
WORKDIR /usr/share/nginx/html
RUN rm -rf ./*
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
CMD ["nginx" , "-g" , "daemon off;"]
```

docker-compose.yml file for Dockerfile above:
```
version: '3.7'

services:
  builder:
    build:
      context: ./
      target: builder
    image: builder-image
  nginx:
    build:
      context: ./
      target: nginx
    image: nginx
    ports:
      - 3000:80
    depends_on:
      - builder
  cypress:
    build:
      context: ./
      target: test-runner
    image: cypress-test-runner
    working_dir: /app
    depends_on:
      - nginx
    network_mode: host
    environment:
      - CYPRESS_baseUrl=http://localhost:3000
      - BASE_IMAGE=builder-image
      - DISPLAY
    entrypoint: cypress open --project /app
```


The most important point is environments.
Run `xhost +` or  `xhost +local:` in your terminal to ensure correct permissions are set
then run `docker-compose up --detach`.
