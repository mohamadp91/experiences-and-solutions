## H2Database
When you create a springboot project that connect to a database. you shouldn't push your 
**_application.properties_** (for security reasons) to run tests successfully in travis-ci you need a database.
H2 database a proper choice.
___
First add H2database to your project then create a directory named resources in test directory.
Finally, create **_appication.properties_** and config a sample H2database there.

```
spring.datasource.url=jdbc:h2:mem:[database name here]
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.properties.hibernate.dialect: org.hibernate.dialect.H2Dialect
spring.datasource.username=spring_user
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8082
```