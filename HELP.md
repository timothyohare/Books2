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

#### Created a Postman collection.
Only has a couple of things working. Need to create tests that run after
the response code.
Supposedly can use [Newman](https://blog.postman.com/newman-v3/) to run it.

3. Creating an OCI image. I was able to create one using `./gradlew bootBuildImage` with the output of 
'docker.io/library/books2:0.0.1-SNAPSHOT' but I don't know where it is stored. Maybe it tried to upload to docker.io?

Had to do 
1. `docker login`
2. `docker tag books2:0.0.1-SNAPSHOT mirgroundhogdaydog/books2:0.0.1-SNAPSHOT`
3. `docker push mirgroundhogdaydog/books2:0.0.1-SNAPSHOT`

So now it is on hub.docker.com. But I can't see it locally with `docker container ls`
`docker run mirgroundhogdaydog/books2:0.0.1-SNAPSHOT`
`docker container ls`
`docker container inspect priceless_mcnulty`
provides the IP address
                    "IPAddress": "172.17.0.2",
and then http://172.17.0.2:8080/api-docs
works!!

#### Able to get in working in ECS
Did it through the console.  
Had to:
1. create a Security Group that allows 8080 from anywhere
2. create a cluster
3. create a service (runs and maintains your desired number of tasks )
   1. specify a service (not a batch) service
   2. specify a SecurityGroup that allows in the required ports e.g. 8080 from anywhere
4. create a task
   1. use the image URI of `mirgroundhogdaydog/books2:0.0.1-SNAPSHOT` and it automatically finds the one on Docker
5. From the task page open the IP address (remember that a new public IP address is assigned each time it is deployed)

#### Debugging
Debugging is managed through application.properties. `logging.level.org.springframework.web: DEBUG`

#### DNS
Investigated ALB (+taget group), Route53 and API Gateway. API Gateway was able to create basic HTTP gateway, provided a randomly generated domain name and that worked. e.g., `https://p7i5jqgs0b.execute-api.us-east-2.amazonaws.com/api-docs`
Then deleted them all to save costs.
When I stopped the task ECS created a new one with a new public IP address. 
Looks like deployments have to be done by service?
Scaled it down to zero by "Updating Service" "Desired tasks" is 0.

### Todo
1. Link it to a DNS entry so that it has a stable entry point. e.g. ohare.id.au/books2
2. Stable storage so new containers can start and have the same storage
3. AWS CDK definition
4. Switch to Fargate Spot
3. Make the functionality work, currently if you store multiple books they are all stored at index 0. Let's try some TDD, write tests in Postman/unit tests and make the functionality work.
4. 
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

