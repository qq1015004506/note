# Spring Boot三大特性

组件自动装配：

1. 激活自动装配：`@EnableAutoConfiguration` 
2. 配置激活内容：`/META-INF/spring.factories` 
3. 实现：`XXXAutoConfiguration`

嵌入式web容器：

1. Web Servlet：Tomcat、Jetty和Undertow
2. Web Reactive：Netty Web Server

生产准备特性：

1. 指标：`/actuator/metrics`
2. 健康检查：`/actuator/health`
3. 外部化配置：`/actuator/configprops`

# Web应用

## 传统Servlet应用:

1. Servlet组件：`Servlet`、`Filter`、`Listener`
2. Servlet注册：Servlet注解、Spring Bean、RegistrationBean
3. 异步非阻塞：异步Servlet、非阻塞Servlet

### servlet组件

* servlet
  * 实现
    * 在servlet类上添加`@WebServlet`注解
    * 继承`HttpServlet`类

  * URL映射

    * 添加`@WebServlet(urlPatterns="/path")`注解进行URL映射

    ```java
    @WebServlet(urlPatterns = "/servlet")
    public class MyServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp)
                throws ServletException, IOException {
            resp.getWriter().write("hello world");
        }
    }
    ```

    

  * 注册

    * `@ServletComponentScan(basePackages = "package path")`

    ``` java
    @SpringBootApplication
    @ServletComponentScan("pers.quzhiyu.learn.springboot.web.servlet")
    public class SpringbootApplication {
        public static void main(String[] args) {
            SpringApplication.run(SpringbootApplication.class, args);
        }
    }
    ```

    

* filter

* listener



### servlet 注册

注册有3种方式，分别是：

#### servlet注解

* `@ServletComponentScan`
  * `@WebServlet`
  * `@WebFilter`
  * `@WebListener`

#### Spring Bean

* `@Bean` + 
  * Servlet
  * Filter
  * Listener

#### RegistrationBean

* `ServletRegistrationBean`
* `FilterRegistrationBean`
* `ServletListenerRegistrationBean`

### 异步非阻塞

#### 异步Servlet

* `javax.servlet.ServletRequest#startAsync()`
* `javax.servlet.AsyncContext`

```java
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        AsyncContext asyncContext = req.startAsync();
        asyncContext.start(()->{
            try {
                resp.getWriter().write("hello world");
                asyncContext.complete();
            } catch (IOException e) {
                e.printStackTrace();
            }
        });
    }
```



#### 非阻塞Servlet

* `javax.servlet.ServletInputStream#setReadListener`
  * `javax.servlet.ReadListener`
* `javax.servlet.ServletOutputStream#setWriteListener`
  * `javax.servlet.WriteListener`

## spring web mvc 应用

### web mvc视图

#### 模板引擎

* Thymeleaf
* Freemarker
* JSP

#### 内容协商

选择更合适的模板引擎

* `ContentNegotiatingViewResolver`

#### 异常处理

* `@ExceptionHandler`

* `HandlerExceptionResolver`

  * `ExceptionHandlerExceptionResolver`

* `BasicErrorController`(SpringBoot)

  生成默认的错误页面

### web mvc rest

#### 资源服务

* `@RequestMapping`
  * `@GetMapping` 、`@PostMapping`、`@PutMapping`、`@DeleteMapping`

#### 资源跨域

* `@CrossOrigin`
* `WebMvcConfigurer#addCrosMappings`
* 传统解决方案（过时了不推荐）
  * IFrame
  * JSONP

#### 服务发现

* HATEOS

### web mvc核心

#### 核心架构 

* `DispatcherServlet`
* `HandlerMapping`
* `HandlerAdapter`
* `ViewResolver`

#### 处理流程

#### 核心组件
