## Run Springboot tests in Travis-CI
```
language: java
jdk:
  - openjdk11

before_install:
  - mvn -N io.takari:maven:wrapper

before_script:
  - mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V

script:
  - mvn test -B
```

### Run integration tests 
To run integration tests with Travis-CI you need H2Database as database.
First go to _src/test/resources/_ then create _application.properties_ and put phrases below in it.
```
spring.datasource.url=jdbc:h2:mem:user-management-system
spring.datasource.username=spring_user
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.properties.hibernate.dialect: org.hibernate.dialect.H2Dialect
spring.jpa.show-sql=true
server.port=8082
```