# Web MVC 核心

## 理解Spring Web MVC 架构

### 基础架构：Servlet

![1556806849728](..\imgs\servlet.png)

* servlet 特点
  * 请求/响应式（request/response）
  * 屏蔽网络通信细节
  * 完整生命周期
* servlet 职责
  * 处理请求
  * 资源管理
  * 视图渲染

servlet设计非常庞杂，按照单职责原则设计并不合理，但作为底层API这样设计又是非常合理的。

于是就改进除了J2EE的核心模式

### 核心架构：前端控制器

![1556806546425](..\imgs\frontController.png)

* 资源：http://www.corej2eepatterns.com/FrontController.htm
* 实现：Spring Web MVC [DispatcherServlet](https://docs.spring.io/spring/docs/1.0.0/javadoc-api/org/springframework/web/servlet/DispatcherServlet.html)



### Spring Web MVC 架构

![1556806472942](..\imgs\springWebMVC.png)

model -> modelAndView，保存计算的结果准备用于渲染

## 认识Spring Web MVC

### spring Framework时代的一般认识

#### 实现Controller

```java
@Controller
public class HelloWorldController {
    @RequestMapping("")
    public String index() {
        return "index";
    }
}
```

#### 配置Web MVC组件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.0.xsd
">
    <context:component-scan base-package="pers.quzhiyu.web" />
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

#### 部署DispatcherServlet

```xml
<web-app>
    <servlet>
        <servlet-name>app</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/app-context.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>app</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

#### 使用可执行Tomcat Maven插件

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.1</version>
    <executions>
        <execution>
            <id>tomcat-run</id>
            <goals>
                <goal>exec-war-only</goal>
            </goals>
            <phase>package</phase>
            <configuration>
                <path></path>
            </configuration>
        </execution>
    </executions>
</plugin>
```

### Spring Framework时代的重新认识

#### Web MVC核心组件

| 组件Bean类型                         | 功能                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| HandlerMapping                       | 映射请求(Request)到处理器(Handler)加上其关联的拦截器(HandlerInterceptor)列表，其映射关系基于不同的`HandlerMapping`实现的一些标准细节。其中两种主要`HandlerMapping`实现，`RequestMappingHandlerMapping`支持标注`@RequestMapping`的方法，`SimpleURLHandlerMapping`维护精确的URI路径与处理器的映射 |
| HandlerAdapter                       | 帮助`DispatcherServlet`调用请求处理器(Handler)，无须关注其中实际的调用细节。比如，调用注解实现Controller需要解析其关联的注解。`HandlerAdapter`的主要目的是为了屏蔽与`DispathcerServlet`之间的诸多细节。 |
| HandlerExceptionResolver             | 解析异常，可能策略是将异常处理映射到其他处理器(Handlers)、或到某个HTML错误页面，或者其他。 |
| ViewResolver                         | 从处理器(Handler)返回字符串类型的逻辑视图名称解析出实际的`View`对象，该对象将渲染后的内容输出到HTTP响应中。 |
| LocaleResolver,LocaleContextResolver | 从客户端解析出`locale`，为其实现国际化视图                   |
| MultipartResolver                    | 解析多部分请求(如Web浏览器文件上传)的抽象实现                |

##### 交互流程

![1556853921549](D:\note\note\imgs\interactionProcess.png)

1. handlerMapping找到对应的controller(Handler)
2. Adapter适配参数并且调用方法，返回ModelAndView
3. ViewResolver解析视图

#### Web MVC 注解驱动

版本依赖：Spring Framework 3.1+

##### 基本配置步骤

注解配置：`@Configuration` （Spring 范式注解）

组件激活：`@EnableWebMVC`（Spring模块装配）

自定义组件：`WebMVCConfigurer`（SpringBean）

##### 常用注解

注册模型属性：`@ModelAttribute`

```java
@ModelAttribute("message")
public String message() {
    return "index message";
}
// 相当于
public String index(Model model) {
    model.addAttribute("message","index message");
    return "index";
}
```

读取请求头内容：`@RequestHeader`

```java
public String acceptLanguage(@RequestHeader("Accept-Language") String acceptLanguage){
    return acceptLanguage;
}
```

读取Cookie：`@CookieValue`

```java
public String jsessionId(@CookieValue("JSESSIONID") String jsessionId){
    return jsessionId;
}
```

校验参数：`@Valid`、`@Validated`

注解处理：`@ExceptionHandler`

切面通知：`@ControllerAdvice`



#### 自动装配

##### servlet SPI

`ServletContainerInitializer`

```java
public class DefaultAnnotationConfigDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        // <init-param> web.xml
        return new Class[]{DispatcherServletConfiguration.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        // DispatcherServlet
        return new Class[0];
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

