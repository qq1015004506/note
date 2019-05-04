# Web MVC REST 应用

## REST 简介

### 架构约束

* 统一接口 (Uniform interface)
* C/S架构 (Client-Server)
* 无状态 (Stateless)
* 可缓存 (Cacheable)
* 分层系统 (Layered System)
* 按需代码 (Code on demand)

#### 统一接口

* 资源识别 (Identification of resources)
  * URI(Uniform Resource Identifier)

* 资源操作 (Manipulation of resource)
  * HTTP verbs: GET、PUT、POST、DELETE
* 字描述消息 (Self-descriptive message)
  * Content-Type
  * MIME-Type
  * Media Type: Application/javascript、text/html
* 超媒体 (HATEOAS)
  * Hypermedia As The Engine Of Application State

## Web MVC REST 支持

### 定义

| 注解              | 说明                                  | 版本 |
| ----------------- | ------------------------------------- | ---- |
| `@Controller`     | 应用控制器注解声明，Spring模式注解    | 2.5+ |
| `@RestController` | 等效于`@Controller` + `@ResponseBody` | 4.0+ |

### 映射

| 注解              | 说明                   | 版本 |
| ----------------- | ---------------------- | ---- |
| `@RequestMapping` | 应用控制器映射注解声明 | 2.5+ |
| `@GetMapping`     | 获取资源               | 4.3+ |
| `@PostMapping`    | 添加资源               | 4.3+ |
| `@PutMapping`     | 修改资源               | 4.3+ |
| `@DeleteMapping`  | 删除资源               | 4.3+ |
| `@PatchMapping`   | 局部修改资源           | 4.3+ |



### 请求

| 注解             | 说明                             | 版本 |
| ---------------- | -------------------------------- | ---- |
| `@RequestParam`  | 获取请求参数                     | 2.5+ |
| `@RequestHeader` | 获取请求头                       | 3.0+ |
| `@CookieValue`   | 获取Cookie值                     | 3.0+ |
| `@RequestBody`   | 获取完整请求主体内容             | 3.0+ |
| `@PathVariable`  | 获取请求路径变量                 | 3.0+ |
| `RequestEntity`  | 获取请求内容(包括请求体和请求头) | 4.1+ |

### 响应

| 注解             | 说明                             | 版本   |
| ---------------- | -------------------------------- | ------ |
| `@ResponseBody`  | 响应主题注解声明                 | 2.5+   |
| `ResponseEntity` | 响应内容（包括响应主体和响应头） | 3.0.2+ |
| `ResponseCookie` | 响应Cookie内容                   | 5.0+   |

### 拦截

| 注解                    | 说明                          | 版本 |
| ----------------------- | ----------------------------- | ---- |
| `@RestControllerAdvice` | `@RestController`注解切面通知 | 4.3+ |
| `HandlerInterceptor`    | 处理方法拦截器                | 1.0  |

### 跨域

| 注解                               | 说明             | 版本 |
| ---------------------------------- | ---------------- | ---- |
| `@CrossOrigin`                     | 资源跨域声明注解 | 4.2+ |
| `CorsFilter`                       | 资源跨域拦截器   | 4.2+ |
| `WebMvcConfigurer#addCorsMappings` | 注册资源跨域信息 | 4.2+ |

