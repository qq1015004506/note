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