---
title: "Spring框架"
yuque_slug: "ocd3u89ipna19b2i"
yuque_id: 230807997
yuque_updated: "2026-10-14T04:54:33.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/ocd3u89ipna19b2i"
---
## ​ 一、什么是 Spring 框架？

**Spring** 是一个开源的、轻量级的 **Java 企业级应用开发框架**，由 Rod Johnson 在 2003 年首次发布。它的目标是简化 Java EE（现 Jakarta EE）开发，解决传统开发中的复杂性问题。

​ 核心理念：**“不要重复造轮子，用配置代替硬编码”**

---

## ​ 二、Spring 的核心目标

1. **解耦**：减少类与类之间的依赖。
2. **简化开发**：提供模板类（如 JdbcTemplate）减少样板代码。
3. **集成第三方框架**：无缝整合 Hibernate、MyBatis、Redis、Kafka 等。
4. **支持 AOP**：实现日志、事务、安全等横切关注点的统一管理。
5. **测试友好**：支持单元测试和集成测试。

---

## ​三、Spring 的七大核心模块（传统划分）

|  |  |
| --- | --- |
| **模块** | **功能** |
| **Spring Core** | 核心容器，提供 IoC 和 Bean 管理 |
| **Spring Context** | 上下文环境，提供国际化、事件机制等 |
| **Spring AOP** | 面向切面编程支持 |
| **Spring DAO** | 数据访问抽象（如 JdbcTemplate） |
| **Spring ORM** | 整合 ORM 框架（如 Hibernate、MyBatis） |
| **Spring Web** | Web 层支持（MVC、REST） |
| **Spring Test** | 单元测试支持 |

⚡ 现代开发中，这些模块大多通过 **Spring Boot** 自动整合。

---

## ​ 四、Spring 的三大核心思想

### 1. **IoC（Inversion of Control）控制反转**

#### ​ 什么是控制反转？

- 传统方式：对象自己 `new` 依赖对象 → 控制权在“自己”手中。
- Spring 方式：对象的创建由 Spring 容器负责，然后“注入”给你 → 控制权“反转”给了容器。

#### ​ 好处：

- 解耦：类不再依赖具体实现，只依赖接口。
- 易于测试：可以轻松替换模拟对象（Mock）。

```java
// 传统方式（耦合）
UserService userService = new UserServiceImpl();

// Spring 方式（解耦）
@Autowired
private UserService userService; // 容器自动注入
```

---

### 2. **DI（Dependency Injection）依赖注入**

DI 是 IoC 的**实现方式**。Spring 通过以下方式注入依赖：

- **构造器注入**（推荐）
- **Setter 注入**
- **字段注入**（@Autowired，不推荐用于生产）

```java
@Service
public class OrderService {
    private final UserService userService;

    // 构造器注入
    public OrderService(UserService userService) {
        this.userService = userService;
    }
}
```

---

### 3. **AOP（Aspect-Oriented Programming）面向切面编程**

#### ​ 什么是 AOP？

有些功能（如日志、事务、权限）会**横跨多个业务方法**，称为“横切关注点”。AOP 允许你把这些逻辑**抽离出来**，在不修改源码的情况下“织入”到目标方法中。

#### 常见应用场景：

- 日志记录
- 事务管理（@Transactional）
- 权限校验
- 性能监控

```java
@Aspect
@Component
public class LogAspect {
    @Around("@annotation(Loggable)")
    public Object log(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("方法开始：" + pjp.getSignature());
        Object result = pjp.proceed();
        System.out.println("方法结束");
        return result;
    }
}
```

---

## ​ 五、Spring 的常用注解（快速记忆）

|  |  |
| --- | --- |
| **注解** | **作用** |
| `@Component` | 通用组件 |
| `@Service` | 业务层组件 |
| `@Repository` | 数据访问层组件 |
| `@Controller` | 控制器（返回视图） |
| `@RestController` | REST 控制器（返回 JSON） |
| `@Autowired` | 自动注入依赖 |
| `@Value` | 注入配置值 |
| `@Configuration` | 配置类 |
| `@Bean` | 声明一个 Bean |
| `@Transactional` | 声明事务 |
| `@Aspect` | 声明切面 |

---

## ​ 六、Spring Boot：Spring 的超级加速器

**Spring Boot** 是 Spring 的扩展，目标是“**快速搭建、开箱即用**”。

### ​ 核心特性：

- **自动配置**：根据依赖自动配置 Bean（如引入 `spring-boot-starter-web` 就自动配置 MVC）
- **起步依赖（Starter）**：简化 Maven/Gradle 依赖管理
- **内嵌服务器**：Tomcat、Jetty 内置，无需部署 WAR
- **Actuator**：生产监控（健康检查、指标收集）

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

---

## ​ 七、Spring 生态体系（全家桶）

|  |  |
| --- | --- |
| **项目** | **用途** |
| **Spring Boot** | 快速开发 |
| **Spring MVC** | Web MVC 框架 |
| **Spring Data** | 统一数据访问（JPA、Redis、MongoDB） |
| **Spring Security** | 安全认证与授权 |
| **Spring Cloud** | 微服务架构（服务发现、网关、熔断） |
| **Spring Batch** | 批处理 |
| **Spring Kafka / RabbitMQ** | 消息队列 |
| **Spring WebFlux** | 响应式编程 |

---

## ​ 八、Spring 的工作流程（简化版）

1. 启动应用 → Spring 容器初始化
2. 扫描 `@Component` 等注解 → 创建 Bean
3. 通过 `@Autowired` 注入依赖（DI）
4. 执行业务逻辑
5. AOP 拦截方法调用，添加额外功能
6. 返回结果

---

## ​ 九、Spring 的优势总结

|  |  |
| --- | --- |
| **优势** | **说明** |
| **低侵入性** | 代码不需要继承特定类 |
| **松耦合** | 依赖通过配置注入 |
| **易于测试** | 支持 Mock 和单元测试 |
| **强大的社区** | 文档丰富，生态庞大 |
| **企业级支持** | 被阿里巴巴、Netflix、LinkedIn 等广泛使用 |

---

## ​十、一句话总结

**Spring = IoC（控制反转） + DI（依赖注入） + AOP（面向切面） + 生态整合**
