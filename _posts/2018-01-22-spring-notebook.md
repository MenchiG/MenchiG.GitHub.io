---
layout: post
title: Spring NoteBook
tags: Spring
category: blog
---

## Spring NoteBook

### Annotation

@EnableBinding(Source.class)

​	Message publisher

@EnableBinding(Sink.class)

@ServiceActivator(inputChannel = Sink.INPUT)

​	Message consumer

@EnableDiscoveryClient

​	Register to Eureka

@Slf4j

​	Simple log, use object log instead of logger factory

@Data

​	lombok for setter and getter

### Parent POM

modules: share dependency Management

​	