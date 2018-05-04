---
layout: post
title: Spring Boot Notebook
key: 10004
tags: Spring Java note
category: blog
---

Some notes for Spring Boot and Spring Cloud<!--more-->


## Annotation

### JPA(Java persistent API)

@Embeddable vs @Entity

In OO relationship

* a dependent object is considered an aggregate or composite association

In relational model

* The dependent object could have its own table

* Its data could be embedded in the independent object's table. The dependent data is included in the independent object's table.

Assume we have **user** and **address**, if query **address** separately is necessary, **address** should stored in another table, otherwise it can be embedded in **user**.

### Message Queue

@EnableBinding(Source.class)

​	Message publisher

@EnableBinding(Sink.class)

@ServiceActivator(inputChannel = Sink.INPUT)

​	Message consumer

### Eureka

Use service name as url instead of practice address and port number

@EnableDiscoveryClient

​	Register to Eureka

### Hystrix

@EnableCircuitBreaker

@HystrixCommand

### Utility

@Slf4j

​	Simple log, use object log instead of logger factory

@Data

​	lombok for setter and getter

## Parent POM

modules: share dependency Management

## URL

* Use service name, do not need port number
* Inject url in yml files (@Value)  need port number

## Debug

### 1.1 mvn clean install

error -> compile time error -> syntax error, dependency issue

### 2.1 service1

#### 2.1.1 Spring configuration issue.

@Autowired Service1 service1 spring startup failed xxx bean cannot be injected

#### 2.1.2 Address used

change port number

#### 2.1.3 Runtime Dependency issue 

ClassNotFound Exception. ClassCastException, NotFoundException

* Look up whether this dependency can be managed by Spring Boot or Spring Cloud	

* check document and change version number

### 3.1  service1, service2. Service1 -> Service 2 Error

Logging and logging

#### 3.1.1 Before service 1 call service 2

do logging

#### 3.1.2 Made call but got error response.

After service call finished, do logging . service2 network issue

#### 3.1.3 ELK

Splunk

ELK = Elastic Search + Logstash + Kibana

log stream -> Kafka -> log message logging service -> data store, indexing, search