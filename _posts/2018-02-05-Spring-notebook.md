---
layout: post
title: Spring Notebook
tags: Spring Java note
category: blog
---

Spring Core <!--more-->

## Scope

### Singleton

Only one instance is ever created and this same shared instance is injected into each collaborating object

Application context, utility class

### Prototype

A brand new bean instance is created, each and every time the prototype is referenced by collaborating beans

### Request

Return a new bean instance per HTTP request

### Session

Return a new bean instance per HTTP session

### Application

Similar to Singleton scope, but Bean is created per ApplicationContext, not ServletContext



## Inversion of Control & Dependency Injection

@Bean or Component scan

@Autowired or @Inject

### Constructor injection (Dependency is enforced)

```java
@Autowired
public YourBeanClass(AnotherBeanClass someBean) {
  this.someBean = someBean;
}
```



### Field injection

```java
@Autowired
private AnotherBeanClass someBean
```



### Setter injection

```java
@Autowired
public void setSomeBean(AnotherBeanClass someBean) {
  this.someBean = someBean;
}
```



**Constructor injection is better.**

1. Check not null

```java
class MyComponent {
  
  private final AnotherBeanClass someBean;
  
  @Inject
  public MyComponent(AnotherBeanClass someBean) {
    //Check not null
    Assert.notNull(someBean, "AnotherBeanClass must not be null!");
    this.someBean = someBean;
  }
  
  public void myMethod() {
    someBean.doSomething();
  }
}
```

2. Testability

Non-constructor injection
```java
AnotherBeanClass someBean = ...;// mock dependency
MyComponent component = new MyComponent();
//Inject dependency by reflection, that is hard
compoent.myMethod();
```

Constructor injection
```java
AnotherBeanClass someBean = ...;// mock dependency
MyComponent component = new MyComponent(someBean);
compoent.myMethod();
```



## Spring Container

### BeanFactory

BeanFactory is represented by ```org.springframework.beans.factory.BeanFactory```. It is the main and the basic way to access the Spring container. Other ways to access the spring container such as ```ApplicationContext```, ```ListableBeanFactory```, ```ConfigurableBeanFactory``` etc are built upon this BeanFactory interface (extends/implements BeanFactory interface)



BeanFactory interface provides basic functionality for the Spring Container

* It provide DI/IOC mechanism for the Spring

* It is built upon Factory Design Pattern

* It loads the beans definitions and their property descriptions from some configuration source(for example, from XML configuration file)

* Instantiates the beans when they are requested like beanfactory_obj.getBean("beanId")

  ​

### Component Scan

@ComponentScan(basePackage="com.xxx.yyy")

@Component



### Concurrency in Spring

Spring has different bean scopes, but all these scopes enforce is when the bean is created.

If multiple threads access the same bean within the scope, then synchronization is necessary.

#### Design Thread Safe Bean

* Make objects immutable
* Stateless
* Think twice before you do synchronization