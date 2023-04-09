# Reference technology

### Why do we need conventions?

We need conventions because we don't want to think more than necessary.
We want to use our mental energy on solving project-specific problems, rather than problems that are common for all projects.

Besides we would like to open the project written by any of our colleagues and 'feel like home'. Read it: "as little surprises as possible". We don't want to familiarise ourselves with 'yet another project specifics', we want to agree on the best conventions one time and use them over and over (with revising those conventions now and then).   


### Core technological stack 

By default, we're using: 

* Java 11
* Lombok - for generating boilerplate code
* Gradle - for building projects and executing tasks
* Spring Core - dependency injection 
* Spring Boot - running an embedded server
* Spring Data - Access to dao layer
* Spring Security - Controlling authentication and authorization
* Hibernate - for Object Relational Mapping
* Postgres - for database 
* Flyway - for managing database migrations
* Redis - for caching
* Kafka - for async. messaging
* Apache Avro - for contract within kafka topics
* Junit - for testing
* Mockito - for mocking
* System test framework - https://github.com/INK-Solutions/system-test-framework - for system testing 

### Integration patterns

* Kafka - for all async. message-like communication 
* REST - for synchronous communication that is not data intense 
* GRPC - for synchronous communication that is data intense