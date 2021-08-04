## Initialize a SpringBoot project with commandline
The init command lets you create a new project by using start.spring.io without leaving the shell, as shown in the following example:
```
spring init --dependencies=web,data-jpa my-project
Using service at https://start.spring.io
Project extracted to '/Users/developer/example/my-project
```
The preceding example creates a my-project directory with a Maven-based project that uses spring-boot-starter-web and spring-boot-starter-data-jpa 
[more details](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-cli.html#cli-init)