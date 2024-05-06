---
layout: post
title: Spring Cache入门使用教程
toc: true
reward: true
date: 2023-08-17 16:38:36
tags: [Cache]
categories: [Java]
---

## Spring Cache入门使用教程

### 什么是Spring Cache？

Spring Cache是Spring框架提供的一个模块，用于在应用程序中实现缓存功能。它允许你将方法的结果缓存起来，以便在后续调用中重复使用，从而提高应用程序的性能。

### 步骤 1：添加依赖

在你的Spring项目中，首先需要添加Spring Cache的依赖。在Maven项目中，你可以在`pom.xml`文件中添加以下依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

### 步骤 2：配置缓存管理器

在Spring Boot应用程序中，默认情况下，会自动配置一个适合的缓存管理器。如果你需要自定义缓存管理器，你可以在配置类中添加如下配置：

```java
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableCaching
public class CacheConfig {
    // 配置其他缓存管理器的Bean，如Redis、Ehcache等
}
```

### 步骤 3：在方法上添加缓存注解

在你想要启用缓存的方法上添加缓存注解，如`@Cacheable`、`@CachePut`和`@CacheEvict`。

- `@Cacheable`：用于标记方法的结果应该被缓存。如果有缓存可用，方法将不会被调用，而是直接从缓存中返回结果。
- `@CachePut`：用于标记方法的结果应该被缓存，并且每次方法被调用时都会执行方法体，并将结果放入缓存。
- `@CacheEvict`：用于标记方法执行后应该清除缓存。

```java
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class MyService {

    @Cacheable("myCache")
    public String getCachedData(String key) {
        // 这里是实际方法逻辑
        return "Cached Data";
    }
}
```

### 步骤 4：配置缓存参数

你可以在`@Cacheable`、`@CachePut`和`@CacheEvict`注解中指定缓存的参数，如缓存的名称、键生成器等。例如：

```java
@Cacheable(value = "myCache", key = "#key")
public String getCachedData(String key) {
    // ...
}
```

### 步骤 5：运行和测试

现在，你的Spring应用程序已经配置了缓存。你可以运行并测试你的方法，观察缓存的工作情况。

### 小结

通过遵循以上步骤，你就可以在Spring应用程序中开始使用缓存功能。记得根据你的应用程序需求和性能优化要求来选择适合的缓存管理器和注解。
