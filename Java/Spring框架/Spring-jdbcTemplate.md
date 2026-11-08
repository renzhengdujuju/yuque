---
title: "Spring-jdbcTemplate"
yuque_slug: "os3kzzov80trouxw"
yuque_id: 231318743
yuque_updated: "2026-10-14T04:54:33.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/os3kzzov80trouxw"
---
```java
package com.eagles.beans;

public class Person {

}
```

```java
package com.eagles.beans;

public class User {
    private  int id;
    private  String username;
    private  String password;
    private  String phone;
    private  String email;
    private  String address;

    public User() {
    }

    public User(int id, String username, String password, String phone, String email, String address) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.phone = phone;
        this.email = email;
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
        "id=" + id +
        ", username='" + username + '\'' +
        ", password='" + password + '\'' +
        ", phone='" + phone + '\'' +
        ", email='" + email + '\'' +
        ", address='" + address + '\'' +
        '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

```java
package com.eagles.config;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.eagles")
public class SpringConfig {

}
```

```java
package com.eagles.user.dao;
import com.eagles.beans.User;
import java.util.List;

public interface UserDao {
    //添加
    int insert(User user);

    //修改
    int update(User user);

    //删除
    int delete(int id);

    //查询
    List<User> findAll();

    //查询某条记录
    User findById(int id);

}
```

```java
package com.eagles.user.dao;
import com.eagles.beans.User;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

public class UserDaoImpl implements UserDao {

    private JdbcTemplate jdbcTemplate;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public int insert(User user) {
        return jdbcTemplate.update("insert into tb_user(username,password,phone,email,address)values (?,?,?,?,?)",user.getUsername(),user.getPassword(),user.getPhone(),user.getEmail(),user.getAddress());
    }

    @Override
    public int update(User user) {
        return jdbcTemplate.update("update tb_user set username=?,password=?,phone=?,email=?,address=? where id=?",user.getUsername(),user.getPassword(),user.getPhone(),user.getEmail(),user.getAddress(),user.getId());
    }

    @Override
    public int delete(int id) {
        return jdbcTemplate.update("delete from tb_user where id = ?",id);
    }

    @Override
    public List<User> findAll() {
        return jdbcTemplate.query("select * from tb_user", BeanPropertyRowMapper.newInstance(User.class));
    }

    @Override
    public User findById(int id) {

        return jdbcTemplate.query("select * from tb_user where id = ?", new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet resultSet, int i) throws SQLException {

                int id = resultSet.getInt("id");
                String password = resultSet.getString("password");
                String username = resultSet.getString("username");
                String email = resultSet.getString("email");
                String phone = resultSet.getString("phone");
                String address = resultSet.getString("address");

                return new User(id,username,password,phone,email,address);
            }
        },id).get(0);
    }
}
```

```java
package com.eagles.user.service;

import com.eagles.beans.User;

import java.util.List;

public interface UserService {

    /**
     * 添加
     */
    int insert(User user);

    /**
     * 修改
     */
    int update(User user);

    /**
     * 删除
     */
    int delete(int id);

    /**
     * 查询
     */
    List<User> findAll();

    /**
     * 查询某一条记录
     */
    User findById(int id);

}
```

```java
package com.eagles.user.service;
import com.eagles.beans.User;
import com.eagles.user.dao.UserDao;

import java.util.List;
public class UserServiceImpl implements  UserService {
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao=userDao;
    }

    @Override
    public int insert(User user) {
        return userDao.insert(user);
    }

    @Override
    public int update(User user) {
        return userDao.update(user);
    }

    @Override
    public int delete(int id) {
        return userDao.delete(id);
    }

    @Override
    public List<User> findAll() {
        return userDao.findAll();
    }

    @Override
    public User findById(int id) {
        return userDao.findById(id);
    }
}
```

```java
package com.spring;

import com.eagles.beans.User;
import com.eagles.config.SpringConfig;
import com.eagles.user.service.UserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

/**
 * @RunWith 该注解的作用是用于替换运行器
 * @ContextConfiguration  用于指定容器的创建方式
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/beans.xml")
//@ContextConfiguration(classes = SpringConfig.class)
public class SpringTest {

    @Autowired
    ApplicationContext applicationContext;
    @Autowired
    UserService userService;

    @Test
    public void test01(){
        Object person = applicationContext.getBean("person");
        System.out.println(person);
    }
    @Test
    public void test02(){
        //创建user对象
        User user = new User();
        user.setUsername("123");
        user.setPassword("123");
        user.setPhone("12311464887");
        user.setEmail("123@qq.com");
        user.setAddress("南京市");

        int num = userService.insert(user);

        System.out.println(num ==1 ? "成功":"失败");
    }

    /**
     * 修改
     */
    @Test
    public void test03(){
        //创建user对象
        User user = new User();
        user.setId(11);
        user.setUsername("132456");
        user.setPassword("123456");
        user.setPhone("111111111");
        user.setEmail("234@qq.com");
        user.setAddress("镇江市");

        int num = userService.update(user);
        System.out.println(num == 1 ? "成功" : "失败");
    }

    /**
     * 删除
     */
    @Test
    public  void test04(){
        int num = userService.delete(11);
        System.out.println(num == 1 ? "成功" :"失败");
    }

    /**
     * 查询
     */
    @Test
    public void test05(){
        List<User> userList = userService.findAll();
        for (User user : userList) {
            System.out.println(user);
        }
    }

    /**
     * 查询某一条记录
     */
    @Test
    public void test06(){
        User user = userService.findById(4);
        System.out.println(user);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd">

  <context:component-scan base-package="com.eagles.beans"></context:component-scan>

  <!--将业务层的实例添加到容器中-->
  <bean id="service" class="com.eagles.user.service.UserServiceImpl">
    <!--注入持久层的实例-->
    <property name="userDao" ref="dao"></property>
  </bean>

  <!--将持久层的实例添加到容器中-->
  <bean id="dao" class="com.eagles.user.dao.UserDaoImpl">
    <!--注入jdbcTemplate-->
    <property name="jdbcTemplate" ref="jdbcTemplate"></property>
  </bean>

  <!--装配jdbcTemplate的实例-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入数据库的实例-->
    <property name="dataSource" ref="dataSoure"></property>
  </bean>

  <!--将person的实例添加到容器中-->
  <bean id="person" class="com.eagles.beans.Person">

  </bean>

  <!--装配数据源的实例-->
  <bean id="dataSoure" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <!--注入连接数据库相关的参数-->
    <property name="url" value="jdbc:mysql://localhost:3306/database1?characterEncoding=utf8&amp;serverTimezone=GMT%2B8"></property>
    <property name="username" value="root"></property>
    <property name="password" value=""></property>
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
  </bean>
</beans>
```

### 1. **IoC（控制反转） / DI（依赖注入）**

- **是什么？**

- IoC 是 Spring 的核心思想：将对象的创建和依赖关系交给 Spring 容器管理，而不是由程序主动创建。
- DI 是 IoC 的实现方式之一，即通过配置或注解自动注入依赖。

- **在你项目中的体现：**

```xml
<bean id="service" class="com.eagles.user.service.UserServiceImpl">
  <property name="userDao" ref="dao"></property>
</bean>
```

- `UserServiceImpl` 依赖 `UserDao` 接口，通过 `<property>` 注入 `UserDaoImpl` 实例。
- 这就是典型的 **基于 XML 的依赖注入（Setter 注入）**。

- **使用场景**：

- 当一个类（如 `UserServiceImpl`）需要另一个类（如 `UserDaoImpl`）来完成工作时，不要手动 `new`，而是让 Spring 帮你注入。

---

### 2. **Bean 管理（XML 配置 vs 注解驱动）**

- **XML 配置方式（当前使用）**

- 所有 Bean（如 `UserDaoImpl`, `JdbcTemplate`, `DataSource`）都在 `beans.xml` 中手动声明。
- 优点：配置集中、结构清晰；缺点：繁琐、不易维护。

- **注解驱动方式（推荐）**

- 使用 `@Component`, `@Service`, `@Repository`, `@Controller` 等注解标记类为 Bean。
- 使用 `@Autowired` 自动注入依赖。
- 配合 `@ComponentScan` 自动扫描包下的组件。

- **你项目中也有注解方式（但被注释了）**

```java
@Configuration
@ComponentScan("com.eagles")
public class SpringConfig { }
```

- **使用场景对比**：

|  |  |
| --- | --- |
| **方式** | **适用场景** |
| XML 配置 | 老项目、需要精细控制 Bean 创建过程 |
| 注解 + Java Config | 新项目、微服务、Spring Boot 主流方式 |

---

### 3. **组件注解（@Component 及其衍生注解）**

- `@Component`：通用组件，任何类都可以用。
- `@Service`：业务层组件（建议 `UserServiceImpl` 用这个）。
- `@Repository`：持久层组件（建议 `UserDaoImpl` 用这个）。
- `@Controller`：Web 层控制器（MVC 中使用）。

你目前只用了 `@Component`，可以优化为更语义化的注解。

---

### 4. **JdbcTemplate（Spring JDBC 模块）**

- **是什么？**

- Spring 提供的轻量级 JDBC 封装，简化数据库操作，避免重复写 `Connection`, `Statement`, `ResultSet` 等样板代码。

- **你项目中的使用：**

```java
jdbcTemplate.update("insert into ...");
jdbcTemplate.query("select * from ...", new RowMapper<User>() { ... });
```

- **使用场景**：

- 不想用 MyBatis/Hibernate，但又不想写原生 JDBC。
- 快速开发小型项目、工具类、脚本任务等。

- 注意：`JdbcTemplate` 仍需手写 SQL，适合对性能要求高、SQL 复杂的场景。

---

### 5. **数据源（DataSource）与数据库连接管理**

- 你使用了 `DriverManagerDataSource`：

```xml
<bean id="dataSoure" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
  <property name="url" value="jdbc:mysql://localhost:3306/datebase1?..." />
  <property name="username" value="root" />
  <property name="password" value="root" />
  <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
</bean>
```

- 问题：`DriverManagerDataSource` 每次都创建新连接，**不适合生产环境**。
- 推荐使用连接池（如 **HikariCP、Druid、C3P0**）：

```xml
<bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
  <property name="jdbcUrl" value="jdbc:mysql://..." />
  <property name="username" value="root" />
  <property name="password" value="root" />
  <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
</bean>
```

---

### 6. **RowMapper（结果集映射）**

- 用于将 `ResultSet` 映射为 Java 对象。
- 你实现了 `RowMapper<User>` 手动映射字段。

```java
new RowMapper<User>() {
    @Override
    public User mapRow(ResultSet rs, int rowNum) throws SQLException {
        return new User(
            rs.getInt("id"),
            rs.getString("username"),
            ...
        );
    }
}
```

- **使用场景**：

- `JdbcTemplate.query()` 查询多条记录时必须使用。
- 可以简化为 `BeanPropertyRowMapper.newInstance(User.class)`（字段名一致时）。

---

### 7. **Spring 单元测试（SpringJUnit4ClassRunner）**

- 使用 `@RunWith(SpringJUnit4ClassRunner.class)` 启动 Spring 容器进行测试。
- `@ContextConfiguration` 指定配置来源（XML 或 Java 类）。
- `@Autowired` 自动注入 `ApplicationContext` 或 `UserService`。

```java
@Autowired
UserService userService;
```

- **使用场景**：

- 测试 Service 是否能正常调用 DAO。
- 验证数据库操作是否成功。
- 集成测试（Integration Test）。

## 一、这个案例讲的是什么？

**核心主题：使用 Spring 容器管理业务对象（Service）和数据访问对象（DAO），并通过 JdbcTemplate 操作数据库，实现用户信息的 CRUD（增删改查）功能。**

通俗地说：

- 以前我们手动 `new UserService()`、`new UserDao()`，还要自己传 `JdbcTemplate`。
- 现在我们把这些对象的创建和依赖关系都交给 **Spring 容器** 来管理。
- 我们只需要写好代码和配置，Spring 会自动帮我们“组装”好所有对象。
- 最终通过 JUnit 测试类调用 Service 层方法，完成对数据库的操作。

这是典型的 **Spring + JDBC + 分层架构** 的入门实战项目。

---

## 二、项目结构概览

```plain
src/
├── com.eagles.beans/           <-- 实体类（POJO）
│   ├── Person.java             <-- @Component 示例
│   └── User.java               <-- 用户实体
│
├── com.eagles.config/          <-- Spring 配置类
│   └── SpringConfig.java       <-- Java Config 配置类
│
├── com.eagles.user.dao/        <-- 数据访问层（DAO）
│   ├── UserDao.java            <-- DAO 接口
│   └── UserDaoImpl.java        <-- DAO 实现类（使用 JdbcTemplate）
│
├── com.eagles.user.service/    <-- 业务逻辑层（Service）
│   ├── UserService.java        <-- Service 接口
│   └── UserServiceImpl.java    <-- Service 实现类（依赖 UserDao）
│
├── com.spring/                 <-- 测试包
│   └── SpringTest.java         <-- JUnit 测试类（集成 Spring）
│
└── resources/xml/beans.xml     <-- Spring XML 配置文件（定义 Bean 及依赖）
```

---

## 三、关键技术点解析

### 1. **分层架构（Layered Architecture）**

|  |  |
| --- | --- |
| **层级** | **作用** |
| `beans.User` | 实体类，对应数据库表 `tb_user` |
| `dao.UserDao` | 数据访问接口，定义数据库操作方法 |
| `dao.UserDaoImpl` | 实现 DAO 接口，使用 `JdbcTemplate`  执行 SQL |
| `service.UserService` | 业务接口，定义业务方法 |
| `service.UserServiceImpl` | 实现 Service 接口，调用 DAO 完成业务逻辑 |

典型的 MVC 后端分层：Controller → Service → DAO → DB

---

### 2. **Spring IoC 容器的作用**

Spring 容器负责：

- 创建 `UserServiceImpl`、`UserDaoImpl`、`JdbcTemplate`、`DataSource` 等对象
- 自动注入它们之间的依赖关系（如：把 `UserDao` 注入到 `UserService` 中）
- 提供统一的入口（`ApplicationContext`）来获取这些对象

---

### 3. **依赖注入（DI）方式：基于 XML 的 setter 注入**

在 `beans.xml` 中通过 `<property>` 完成依赖注入：

```xml
<!-- Service 依赖 UserDao -->
<bean id="service" class="UserServiceImpl">
  <property name="userDao" ref="dao"/>
</bean>

<!-- Dao 依赖 JdbcTemplate -->
<bean id="dao" class="UserDaoImpl">
  <property name="jdbcTemplate" ref="jdbcTemplate"/>
</bean>

<!-- JdbcTemplate 依赖 DataSource -->
<bean id="jdbcTemplate" class="JdbcTemplate">
  <property name="dataSource" ref="dataSoure"/>
</bean>

<!-- 数据源配置 -->
<bean id="dataSoure" class="DriverManagerDataSource">
  <property name="url" value="jdbc:mysql://..."/>
  <property name="username" value="root"/>
  <property name="password" value="root"/>
  <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
</bean>
```

相当于：

```java
DataSource ds = new DriverManagerDataSource(...);
JdbcTemplate jt = new JdbcTemplate();
jt.setDataSource(ds);

UserDaoImpl dao = new UserDaoImpl();
dao.setJdbcTemplate(jt);

UserServiceImpl service = new UserServiceImpl();
service.setUserDao(dao);
```

但这一切都由 Spring 自动完成！

---

### 4. **JdbcTemplate 的作用**

- Spring 提供的 JDBC 封装工具类
- 简化了 JDBC 编程：自动处理连接、异常、资源释放等
- 支持 SQL 查询、更新、批处理等
- 使用 `RowMapper` 将 `ResultSet` 映射为 Java 对象

#### 示例：查询所有用户

```java
@Override
public List<User> findAll() {
    return jdbcTemplate.query("select * from tb_user", new RowMapper<User>() {
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            return new User(
                rs.getInt("id"),
                rs.getString("username"),
                rs.getString("password"),
                rs.getString("phone"),
                rs.getString("email"),
                rs.getString("address")
            );
        }
    });
}
```

---

### 5. **配置方式对比：XML vs Java Config**

#### 当前使用的是 XML 配置：

```java
@ContextConfiguration(locations = "classpath:xml/beans.xml")
```

#### 也可以使用 Java 配置（注释掉的部分）：

```java
@ContextConfiguration(classes = SpringConfig.class)
```

其中 `SpringConfig.java` 是：

```java
@Configuration
@ComponentScan("com.eagles")
public class SpringConfig {}
```

但注意：虽然加了 `@ComponentScan`，但 `UserServiceImpl` 和 `UserDaoImpl` 并没有加 `@Service` 或 `@Repository` 注解，所以仅靠 `@ComponentScan` 是无法自动注册它们的！

所以目前只能使用 XML 配置来显式声明 Bean。

---

### 6. **测试类 SpringTest.java 解析**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/beans.xml")
public class SpringTest {

    @Autowired
    ApplicationContext applicationContext;

    @Autowired
    UserService userService; // Spring 自动注入 UserServiceImpl

    @Test
    public void test02() {
        User user = new User();
        user.setUsername("123");
        // ... 设置其他属性
        int num = userService.insert(user);
        System.out.println(num == 1 ? "成功" : "失败");
    }
}
```

- 使用 JUnit + Spring 测试框架
- Spring 容器启动后，自动注入 `UserService`
- 直接调用 `userService.insert()` 方法即可操作数据库

---

## 四、运行流程图解

```plain
JUnit 启动
    ↓
Spring 容器初始化（加载 beans.xml）
    ↓
创建 Bean：
   1. DriverManagerDataSource（数据源）
   2. JdbcTemplate（注入 DataSource）
   3. UserDaoImpl（注入 JdbcTemplate）
   4. UserServiceImpl（注入 UserDao）
    ↓
@Autowired 注入 userService 到测试类
    ↓
执行 test02() → userService.insert(user)
    ↓
UserServiceImpl 调用 userDao.insert(user)
    ↓
UserDaoImpl 使用 jdbcTemplate.update(...) 执行 SQL
    ↓
数据写入 MySQL 数据库
```

---

## 五、这个案例教会我们什么？

你学会了：

1. 如何使用 Spring 管理多层架构对象（Service、DAO）
2. 如何通过 XML 配置实现依赖注入（setter 注入）
3. 如何使用 `JdbcTemplate` 简化 JDBC 编程
4. 如何使用 `RowMapper` 将结果集映射为 Java 对象
5. 如何配置 `DataSource` 连接 MySQL 数据库
6. 如何使用 JUnit + Spring 进行集成测试
7. 理解 IoC/DI 的实际应用场景

---

## 六、可以改进的地方（进阶建议）

|  |  |
| --- | --- |
| **问题** | **改进建议** |
| DAO/Service 没有使用注解 | 给 `UserServiceImpl`  加 `@Service`  ，`UserDaoImpl`  加 `@Repository`  ，然后用 `@ComponentScan`  替代 XML |
| 数据库配置硬编码 | 使用 `jdbc.properties`  文件 + `<context:property-placeholder>` |
| 没有事务管理 | 添加 `@Transactional`  和 `<tx:annotation-driven/>` |
| 没有连接池 | 使用 `HikariCP`  或 `Druid`  替代 `DriverManagerDataSource` |
| 没有日志 | 添加 `logback`  或 `slf4j`  输出 SQL 日志 |

##
