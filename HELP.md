# Getting Started

## Custom notes
start with ./gradlew bootRun
call APIs with the trailing slash e.g., http://localhost:8080/api/book/

The "hidden" rest controller works without the trailing slash
http://localhost:8080/getTime

Links to
https://github.com/eugenp/tutorials/blob/master/spring-boot-modules/spring-boot-springdoc/src/main/java/com/baeldung/springdoc/controller/BookController.java
and https://www.baeldung.com/spring-rest-openapi-documentation

### Creating books
`curl -X POST http://localhost:8080/api/book/ -H 'Content-Type: application/json'`
`-d '{"title":"Starcraft 2.0","author":"Stephen Hitchens"}'`

Retrieving the created books.
`curl -X GET http://localhost:8080/api/book/` and
`curl -X GET http://localhost:8080/api/book/0` do not provide the results.
Need to figure out how to debug in IntelliJ
(I can set breakpoints but can't inspect variables)
and to log messages.  
Was able to get the books saved by adding `repository.add(book);` to the POST mapping.  
Was able to add debug logging by `logging.level.org.springframework.web: DEBUG` to application.properties
1. Get documentation working, I was following
   https://www.baeldung.com/spring-rest-openapi-documentation but none of the endpoints were working for me.
   Can't load `//implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.1.0'`
   Following https://stackoverflow.com/questions/74614369/how-to-run-swagger-3-on-spring-boot-3
   https://springdoc.org/

had to replace org.springdoc.api.annotations.ParameterObject
with
org.springdoc.core.annotations.ParameterObject
as per https://springdoc.org/#migrating-from-springdoc-v1

Now the following works:
* http://localhost:8080/swagger-ui/index.html
* http://localhost:8080/api-docs
  as per the settings in application.properties.
### Todo

3. Creating an OCI image. I was able to create one using `./gradlew bootBuildImage` with the output of 
'docker.io/library/books2:0.0.1-SNAPSHOT' but I don't know where it is stored. Maybe it tried to upload to docker.io?


### Reference Documentation
For further reference, please consider the following sections:

* [Official Gradle documentation](https://docs.gradle.org)
* [Spring Boot Gradle Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.1.1/gradle-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.1.1/gradle-plugin/reference/html/#build-image)
* [Spring Web](https://docs.spring.io/spring-boot/docs/3.1.1/reference/htmlsingle/#web)

### Guides
The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

### Additional Links
These additional references should also help you:

* [Gradle Build Scans – insights for your project's build](https://scans.gradle.com#gradle)

