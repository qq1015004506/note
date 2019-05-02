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

> 推断的目的：让用户减少配置
>
> ```java
> private WebApplicationType deduceWebApplicationType() {
>     //如果存在Reactive的DispatcherServlet不存在servlet的DispatcherServlet
>     //就返回 WebApplicationType.REACTIVE，该程序是 REACTIVE 程序
>     if (ClassUtils.isPresent(REACTIVE_WEB_ENVIRONMENT_CLASS, null)
>         && !ClassUtils.isPresent(MVC_WEB_ENVIRONMENT_CLASS, null)) {
>         return WebApplicationType.REACTIVE; 
>     }
>     //如果都不存在就返回 WebApplicationType.NONE,不是WEB程序
>     for (String className : WEB_ENVIRONMENT_CLASSES) {
>         if (!ClassUtils.isPresent(className, null)) {
>             return WebApplicationType.NONE;   //该程序为 NONE
>         }
>     }
>     //WebMVC和Webflux同时存在时激活WebMVC
>     //默认返回是 SERVLET 程序
>     return WebApplicationType.SERVLET;    
> }
> ```

也可以通过`setWebApplicationType(WebApplicationType.NONE);`关闭掉WEB服务

### 推断引导类

根据Main线程执行堆栈判断实际的引导类，原理是当异常产生时会逐层上抛，最终到Main函数。

因此通过堆栈能找到引导类

如果通过`springApplication.setSource(Set<String> source)` 方式设置引导类就并不知道main引导类

```java
private Class<?> deduceMainApplicationClass() {
    try {
        StackTraceElement[] stackTrace = (new RuntimeException()).getStackTrace();
        StackTraceElement[] var2 = stackTrace;
        int var3 = stackTrace.length;

        for(int var4 = 0; var4 < var3; ++var4) {
            StackTraceElement stackTraceElement = var2[var4];
            if ("main".equals(stackTraceElement.getMethodName())) {
                return Class.forName(stackTraceElement.getClassName());
            }
        }
    } catch (ClassNotFoundException var6) {
        ;
    }

    return null;
}
```

### 加载应用上下文初始器

利用Spring工厂加载机制，实例化`ApplicationContextInitializer`实现类，并排序对象集合。

* 实现类：`org.springframework.core.io.support.SpringFactoriesLoader`

  ```java
  private <T> Collection<T> getSpringFactoriesInstances(Class<T> type, 
      Class<?>[] parameterTypes, Object... args) {
      ...
      //classpath中有多份文件加载完资源后对资源顺序进行排列
      AnnotationAwareOrderComparator.sort(instances);
      return instances;
  }
  ```

  

* 配置资源：`META-INF/spring.factories`

  ```
  org.springframework.context.ApplicationContextInitializer=\
  pers.quzhiyu.learn.springboot.context.AfterInitializer,\
  pers.quzhiyu.learn.springboot.context.HelloInitializer  
  ```

* 排序：`AnnotationAwareOrderComparator#sort`

  ```java
  @Order(Ordered.HIGHEST_PRECEDENCE)
  public class HelloInitializer implements ApplicationContextInitializer {
      @Override
      public void initialize(ConfigurableApplicationContext context) {
          System.out.println("hello Highest precedence...");
      }
  }
  @Order(Ordered.LOWEST_PRECEDENCE)
  public class AfterInitializer implements ApplicationContextInitializer {
      @Override
      public void initialize(ConfigurableApplicationContext context) {
          System.out.println("after Highest precedence...");
      }
  }
  
  ```

  

  

## SpringApplication 运行阶段

### 加载运行监听器`SpringApplicationRunListeners`

利用Spring工厂加载机制，读取`SpringApplicationRunListener`对象集合，并且封装到组合类`SpringApplicationRunListeners`





















