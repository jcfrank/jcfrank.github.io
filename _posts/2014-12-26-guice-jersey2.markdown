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
this dependency, too.

```xml
<dependency>
    <groupId>org.glassfish.hk2</groupId>
    <artifactId>guice-bridge</artifactId>
    <version>2.3.0</version>
</dependency>
```

For Jersey 2, include jersey-container-servlet dependency, and extends
Jersey's ResourceConfig.

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
But this is the tricky part that took me a lot of time.  
