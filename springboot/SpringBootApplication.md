# Spring Application

* 定义：Spring应用引导类，提供便利的自定义行为方法
* 场景：嵌入式Web应用和非Web应用
* 运行：`SpringApplication#run(Args)`

## `SpringApplication`基本使用

### `SpringApplication`运行

```java
SpringApplication.run(SpringbootApplication.class,args);
```

### 自定义`SpringApplication`

#### 通过`SpringApplication` API调整

```java
SpringApplication springApplication = new SpringApplication(SpringbootApplication.class);
springApplication.setBannerMode(Banner.Mode.CONSOLE);
springApplication.setWebApplicationType(WebApplicationType.NONE);
springApplication.setAdditionalProfiles("prod");
springApplication.setHeadless(true);
```

#### 通过`SpringApplicationBuilder`API调整

```java
new SpringApplicationBuilder(SpringbootApplication.class)
        .bannerMode(Banner.Mode.CONSOLE)
        .web(WebApplicationType.NONE)
        .profiles("prod")
        .headless(true)
        .run(args);
```

## Spring Boot 准备阶段

### 配置 Spring Boot Bean 源

Java配置Class或XML上下文配置文件集合，用于Spring Boot`BeanDefinitionLoader`读取，并且将配置源解析加载为Spring Bean定义

* 数量：一个或多个。

#### Java配置Class

用于Spring注解驱动中java配置类，大多数情况是Spring模式注解所标注的类，如`@Configuration`。(标注了`@Component`或其衍生类的注解的类)

#### XML上下文配置文件

用于Spring传统配置驱动中的xml文件

### 推断应用类型

根据当前应用的ClassPath中是否存在相关类来推断Web应用类型，包括：

* web Reactive：`WebApplicationType.REACTIVE`
* Web Servlet：`WebApplicationType.SERVLET`
* 非 Web：`WebApplicationType.NONE`

### 推断引导类

根据Main线程执行堆栈判断实际的引导类

### 加载应用上下文初始器

利用Spring工厂加载机制，实例化`ApplicationContextInitializer`实现类，并排序对象集合。

* 实现类：`org.springframework.core.io.support.SpringFactoriesLoader`
* 配置资源：`META-INF/spring.factories`
* 排序：`AnnotationAwareOrderComparator#sort`

