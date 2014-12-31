---
layout: post
title: "Google Guice and Jersey 2"
date: 2014-12-26
comments: true
categories: programming
tags: programming java guice jersey
---

It's a bit tricky to have Guice and Jersey 2 work together.
Generally, if we want to use Dependency Injection with Jersey 2, Spring is a better
choice.

Both of them have solutions to integrate with Jersey 2, but the Spring
one is better documented.

How to
------

In pom.xml, include guice and guice-servlet dependencies. Then add
guice-bridge dependency, too.

```xml
<dependency>
    <groupId>org.glassfish.jersey.containers</groupId>
    <artifactId>jersey-container-servlet</artifactId>
    <version>${jersey.version}</version>
</dependency>
...
<dependency>
    <groupId>com.google.inject.extensions</groupId>
    <artifactId>guice-servlet</artifactId>
    <version>3.0</version>
</dependency>
<dependency>
    <groupId>org.glassfish.hk2</groupId>
    <artifactId>guice-bridge</artifactId>
    <version>2.3.0</version>
</dependency>
...
```

For Jersey 2, include jersey-container-servlet dependency, and extends
Jersey's ResourceConfig. Inject ServiceLocator, note that the Inject
annotation must use the one in javax package.
Provide modules here.

```java
@Inject
public MyApplication(ServiceLocator serviceLocator) {
    packages(true, "my.resource.package");

    GuiceBridge.getGuiceBridge().initializeGuiceBridge(serviceLocator);
    GuiceIntoHK2Bridge guiceBridge =
        serviceLocator.getService(GuiceIntoHK2Bridge.class);
    guiceBridge.bridgeGuiceInjector(
        Guice.createInjector(Stage.PRODUCTION,
            new MyModule()
        ));
}
```

Of course other Jersey related configurations and Guice filters in
web.xml all need to be set.

```xml
<filter>
    <filter-name>guiceFilter</filter-name>
    <filter-class>com.google.inject.servlet.GuiceFilter</filter-class>
    <async-supported>true</async-supported>
</filter>

<filter-mapping>
    <filter-name>guiceFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

After these settings the Guice injections should be working with Jersey
2.  

Why do we need these steps for Guice to work on Jersey 2
--------------------------------------------------------

Jersey also uses dependency injection to populate resource instances. In Jersey 2, it's HK2.  
So when resource instances are required by the servlet container, it asks HK2's ServiceLocator.  
This ServiceLocator doesn't know what we defined in Guice modules.  
That's basically the reason we get null when we try to inject implementations defined in Guice modules to Jersey resources.  

I haven't look into Guice-Bridge to understand what it does. But probably something like providing Guice Injector's objects to HK2 ServiceLocator.  

