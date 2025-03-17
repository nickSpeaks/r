## Project: Spring Social Media API
Below are the users stories that must be completed:

### User Registration
As a user, I should be able to create a new Account on the endpoint POST localhost:8080/register. The body will contain a representation of a JSON Account, but will not contain an account_id.
* The registration will be successful if and only if the username is not blank, the password is at least 4 characters long, and an Account with that username does not already exist. If all these conditions are met, the response body should contain a JSON of the Account, including its account_id. The response status should be 200 OK, which is the default. The new account should be persisted to the database.
* If the registration is not successful due to a duplicate username, the response status should be 409. (Conflict)
* If the registration is not successful for some other reason, the response status should be 400. (Client error)

### Login
As a user, I should be able to verify my login on the endpoint POST localhost:8080/login. The request body will contain a JSON representation of an Account, not containing an account_id. In the future, this action may generate a Session token to allow the user to securely use the site. We will not worry about this for now.
* The login will be successful if and only if the username and password provided in the request body JSON match a real account existing on the database. If successful, the response body should contain a JSON of the account in the response body, including its account_id. The response status should be 200 OK, which is the default.
* If the login is not successful, the response status should be 401. (Unauthorized)

### Create New Message
As a user, I should be able to submit a new post on the endpoint POST localhost:8080/messages. The request body will contain a JSON representation of a message, which should be persisted to the database, but will not contain a message_id.
* The creation of the message will be successful if and only if the message_text is not blank, is under 255 characters, and posted_by refers to a real, existing user. If successful, the response body should contain a JSON of the message, including its message_id. The response status should be 200, which is the default. The new message should be persisted to the database.
* If the creation of the message is not successful, the response status should be 400. (Client error)

### Get All Messages
As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/messages.
* The response body should contain a JSON representation of a list containing all messages retrieved from the database. It is expected for the list to simply be empty if there are no messages. The response status should always be 200, which is the default.

### Get One Message Given Message Id
As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/messages/{message_id}.
* The response body should contain a JSON representation of the message identified by the message_id. It is expected for the response body to simply be empty if there is no such message. The response status should always be 200, which is the default.

### Delete a Message Given Message Id
As a User, I should be able to submit a DELETE request on the endpoint DELETE localhost:8080/messages/{message_id}.
* The deletion of an existing message should remove an existing message from the database. If the message existed, the response body should contain the number of rows updated (1). The response status should be 200, which is the default.
* If the message did not exist, the response status should be 200, but the response body should be empty. This is because the DELETE verb is intended to be idempotent, ie, multiple calls to the DELETE endpoint should respond with the same type of response.

### Update Message Given Message Id
As a user, I should be able to submit a PATCH request on the endpoint PATCH localhost:8080/messages/{message_id}. The request body should contain a new message_text values to replace the message identified by message_id. The request body can not be guaranteed to contain any other information.
* The update of a message should be successful if and only if the message id already exists and the new message_text is not blank and is not over 255 characters. If the update is successful, the response body should contain the number of rows updated (1), and the response status should be 200, which is the default. The message existing on the database should have the updated message_text.
* If the update of the message is not successful for any reason, the response status should be 400. (Client error)

### Get All Messages From User Given Account Id
As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/accounts/{account_id}/messages.
* The response body should contain a JSON representation of a list containing all messages posted by a particular user, which is retrieved from the database. It is expected for the list to simply be empty if there are no messages. The response status should always be 200, which is the default

### Project leverages Spring Boot Framework
As a developer, I see that the project uses the Spring framework to inject dependencies and autowire functionality using Spring annotations.

#### Project Structure

    main/java/com/example
        controller
        entity
        exception
        repository
        service
        SocialMediaApp.java
    resources
        application.properties
        data.sql

#### Project Requirements
README.md

##### Background

Full-stack applications are typically concerned with both a front end, that displays information to the user and takes in input, and a backend, that manages persisted information.

This project will be a backend for a hypothetical social media app, where we must manage our users’ accounts as well as any messages that they submit to the application. However, the functionality for this project will leverage a popular web application framework for Java known as Spring. The Spring framework allows for automatic injection and configuration of many features, including data persitence, endpoints and conventional data manipulation logic (CRUD operations).

In our hypothetical micro-blogging or messaging app, any user should be able to see all of the messages posted to the site, or they can see the messages posted by a particular user. In either case, we require a backend which is able to deliver the data needed to display this information as well as process actions like logins, registrations, message creations, message updates, and message deletions.

##### Database Tables

The following tables will be initialized in your project's built-in database upon startup using the configuration details in the application.properties file and the provided SQL script.

Account

    accountId integer primary key auto_increment,
    username varchar(255) not null unique,
    password varchar(255)

Message

    messageId integer primary key auto_increment,
    postedBy integer,
    messageText varchar(255),
    timePostedEpoch long,
    foreign key (postedBy) references Account(accountId)

##### Spring Technical Requirement

##### Project must leverage the Spring Boot Framework

Java classes have been provided, but your entire project MUST leverage the Spring framework.
In addition to functional test cases, "SpringTest" will verify that you have leveraged the Spring framework, Spring Boot, Spring MVC, and Spring Data.
SpringTest will verify the following

- That you have, by any means, have a bean for the AccountService, MessageService, AccountRepository, MessageRepository, and SocialMediaController classes
- That AccountRepository and MessageRepository are working JPARepositories based on their corresponding Account and Message entities
- That your Spring Boot app leverages MVC by checking for Spring's default error message structure.

The app will already be a Spring Boot app with a valid application.properties and valid database entities at the start.

##### User Stories

##### 1: Our API should be able to process new User registrations.

As a user, I should be able to create a new Account on the endpoint POST localhost:8080/register. The body will contain a representation of a JSON Account, but will not contain an accountId.

- The registration will be successful if and only if the username is not blank, the password is at least 4 characters long, and an Account with that username does not already exist. If all these conditions are met, the response body should contain a JSON of the Account, including its accountId. The response status should be 200 OK, which is the default. The new account should be persisted to the database.
- If the registration is not successful due to a duplicate username, the response status should be 409. (Conflict)
- If the registration is not successful for some other reason, the response status should be 400. (Client error)

##### 2: Our API should be able to process User logins.

As a user, I should be able to verify my login on the endpoint POST localhost:8080/login. The request body will contain a JSON representation of an Account.

- The login will be successful if and only if the username and password provided in the request body JSON match a real account existing on the database. If successful, the response body should contain a JSON of the account in the response body, including its accountId. The response status should be 200 OK, which is the default.
- If the login is not successful, the response status should be 401. (Unauthorized)


##### 3: Our API should be able to process the creation of new messages.

As a user, I should be able to submit a new post on the endpoint POST localhost:8080/messages. The request body will contain a JSON representation of a message, which should be persisted to the database, but will not contain a messageId.

- The creation of the message will be successful if and only if the messageText is not blank, is not over 255 characters, and postedBy refers to a real, existing user. If successful, the response body should contain a JSON of the message, including its messageId. The response status should be 200, which is the default. The new message should be persisted to the database.
- If the creation of the message is not successful, the response status should be 400. (Client error)

##### 4: Our API should be able to retrieve all messages.

As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/messages.

- The response body should contain a JSON representation of a list containing all messages retrieved from the database. It is expected for the list to simply be empty if there are no messages. The response status should always be 200, which is the default.

##### 5: Our API should be able to retrieve a message by its ID.

As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/messages/{messageId}.

- The response body should contain a JSON representation of the message identified by the messageId. It is expected for the response body to simply be empty if there is no such message. The response status should always be 200, which is the default.

##### 6: Our API should be able to delete a message identified by a message ID.

As a User, I should be able to submit a DELETE request on the endpoint DELETE localhost:8080/messages/{messageId}.

- The deletion of an existing message should remove an existing message from the database. If the message existed, the response body should contain the number of rows updated (1). The response status should be 200, which is the default.
- If the message did not exist, the response status should be 200, but the response body should be empty. This is because the DELETE verb is intended to be idempotent, ie, multiple calls to the DELETE endpoint should respond with the same type of response.

##### 7: Our API should be able to update a message text identified by a message ID.
As a user, I should be able to submit a PATCH request on the endpoint PATCH localhost:8080/messages/{messageId}. The request body should contain a new messageText values to replace the message identified by messageId. The request body can not be guaranteed to contain any other information.

- The update of a message should be successful if and only if the message id already exists and the new messageText is not blank and is not over 255 characters. If the update is successful, the response body should contain the number of rows updated (1), and the response status should be 200, which is the default. The message existing on the database should have the updated messageText.
- If the update of the message is not successful for any reason, the response status should be 400. (Client error)

##### 8: Our API should be able to retrieve all messages written by a particular user.
As a user, I should be able to submit a GET request on the endpoint GET localhost:8080/accounts/{accountId}/messages.

- The response body should contain a JSON representation of a list containing all messages posted by a particular user, which is retrieved from the database. It is expected for the list to simply be empty if there are no messages. The response status should always be 200, which is the default.

##### 9: The Project utilizes the Spring Framework.
- The project was created leveraging the spring framework, including dependency injection, autowire functionality and/or Spring annotations.

Good luck!

### controller

    package com.example.controller;

    /**
    * TODO: You will need to write your own endpoints and handlers for your controller using Spring. The endpoints you will need can be
      * found in readme.md as well as the test cases. You be required to use the @GET/POST/PUT/DELETE/etc Mapping annotations
      * where applicable as well as the @ResponseBody and @PathVariable annotations. You should
      * refer to prior mini-project labs and lecture materials for guidance on how a controller may be built.
      */

    public class SocialMediaController {
    
    }

#### entity

Account.java: This is a class that models an Account. You should NOT make any modifications to this class.

Message.java: This is a class that models a Message. You should NOT make any modifications to this class.

#### exception.txt

It is good practice, although not necessary, to handle unexpected events in your API using a custom exception in this folder. This file exists to populate the containing folder in a git repository.

#### repository

AccountRepository.java

    package com.example.repository;
    
    public interface AccountRepository {
    }

MessageRepository.java

    package com.example.repository;
    
    public interface MessageRepository {
    }

#### service

AccountService.java

    package com.example.service;

    public class AccountService {
    }

MessageService.java

    package com.example.service;
    
    public class MessageService {
    }

#### SocialMediaApp.java

![](src/ProjectSocialMediaApp.png)

#### application.properties

    spring.datasource.url=jdbc:h2:mem:testdb
    spring.datasource.driverClassName=org.h2.Driver
    spring.datasource.username=sa
    spring.datasource.password=password
    spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
    spring.jpa.defer-datasource-initialization=true
    spring.h2.console.enabled=true
    spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl

#### data.sql

    drop table if exists message;
    drop table if exists account;

    create table account (
        accountId int primary key auto_increment,
        username varchar(255) not null unique,
        password varchar(255)
    );
    create table message (
        messageId int primary key auto_increment,
        postedBy int,
        messageText varchar(255),
        timePostedEpoch bigint,
        foreign key (postedBy) references account(accountId)
    );
    
    -- Starting test values with ids of 9999 to avoid test issues

    insert into account values (9999, 'testuser1', 'password');
    insert into account values (9998, 'testuser2', 'password');
    insert into account values (9997, 'testuser3', 'password');
    insert into account values (9996, 'testuser4', 'password');
    
    insert into message values (9999, 9999,'test message 1',1669947792);
    insert into message values (9997, 9997,'test message 2',1669947792);
    insert into message values (9996, 9996,'test message 3',1669947792);

#### pom.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--    project metadata-->
    <groupId>org.revature</groupId>
    <artifactId>Challenges</artifactId>
    <version>1.1</version>
    
      <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.5</version>
      </parent>
    
      <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
      </properties>
    
      <dependencies>
    
    <!--    <dependency>-->
    <!--      <groupId>org.springframework</groupId>-->
    <!--      <artifactId>spring-core</artifactId>-->
    <!--      <version>5.3.23</version>-->
    <!--    </dependency>-->
    <!--    <dependency>-->
    <!--      <groupId>org.springframework</groupId>-->
    <!--      <artifactId>spring-beans</artifactId>-->
    <!--      <version>5.3.23</version>-->
    <!--    </dependency>-->
    <!--    <dependency>-->
    <!--      <groupId>org.springframework</groupId>-->
    <!--      <artifactId>spring-context</artifactId>-->
    <!--      <version>5.3.23</version>-->
    <!--    </dependency>-->
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
    <!--    <dependency>-->
    <!--      <groupId>mysql</groupId>-->
    <!--      <artifactId>mysql-connector-java</artifactId>-->
    <!--    </dependency>-->
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
        </dependency>
    
        <!-- https://mvnrepository.com/artifact/com.h2database/h2 -->
        <dependency>
          <groupId>com.h2database</groupId>
          <artifactId>h2</artifactId>
          <version>2.1.214</version>
        </dependency>
      </dependencies>
    
      <build>
        <plugins>
          <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.5.5</version>
          </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
      </build>
    </project>
