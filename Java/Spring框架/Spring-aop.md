---
title: "Spring-aop"
yuque_slug: "ag36sqca8hy8mv5w"
yuque_id: 231318767
yuque_updated: "2026-10-14T04:54:33.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/ag36sqca8hy8mv5w"
---
SpringAop

AOP：全称是 Aspect Oriented Programming 即：**面向切面编 程**

AOP 的作用及优势 作用：

**在程序运行期间**，**不修改源码对已有方法进行增强**

优势：

**减少重复代码**

**提高开发效率**

**维护方便**

spring中的Aop

Joinpoint(连接点):

连接点是指那些被拦截到的点。在 spring 中,这些点指的是方

法,因为 spring 只支持方法类型的连接点。

Pointcut(切入点):

所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义

Advice(通知/增强):

所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。

通知的类型：前置通知,后置通知,异常通知,最终通知,环绕通知

Target(目标对象):

代理的目标对象。

Weaving(织入):

是指把增强应用到目标对象来创建新的代理对象的过程。spring 采用动态代理织入，而 AspectJ 采用编译期织入和类

装载期织入。

Proxy（代理）:

一个类被 AOP 织入增强后，就产生一个结果代理类。

Aspect(切面):

**是切入点（要增强的方法）和通知（怎么增强）之间的关系**。

Spring **基于**XML**的**AOP**配置**

aop:config：

作用：用于声明开始 aop 的配置

aop:aspect：

作用：配置切面

aop:pointcut：

作用：配置切入点表达式

aop:before：

作用：用于配置前置通知

aop:after-returning：

作用：用于配置后置通知

aop:after-throwing

作用：用于配置异常通知

aop:after

作用：用于配置最终通知

aop:around：

作用：用于配置环绕通知

Spring**基于注解的** AOP **配置**

@Before

@AfterReturning

@AfterThrowing

@After

@Around

@PointcutSpring **中的事务控制**

JavaEE 体系进行分层开发，事务处理位于业务层，Spring 提供了

分层设计业务层的事务处理解决方案

spring 框架为我们提供了一组事务控制的接口。这组接口是在

spring-tx-5.0.2.RELEASE.jar 中

spring **的事务控制都是基于** AOP **的**，它既可以使用编程的方式实

现，也可以使用配置的方式实现。我们学习的重点是使用配置的方式

实现

PlatformTransactionManager接口

此接口是 spring 的事务管理器，它里面提供了我们常用的操作事

务的方法

管理事务的子类对象：

**DataSourceTransactionManager** 使用 SpringJDBC 或 iBatis

进行持久化数据时使用

HibernateTransactionManager 使用Hibernate 版本进行持久化

数据时使用

TransactionDefinition

获取事务的隔离级别

获取事务的传播行为

获取超时时间

获取是否是只读事务

基于 XML 的声明式事务控制

第一步：配置事务管理器

第二步：配置事务的通知引用事务管理器

**第三步：配置事务的属性**

第四步：配置 AOP 切入点表达式

第五步：配置**切入点表达式**和**事务通知**的对应关系

基于注解的配置方式

第一步：配置事务管理器并注入数据源

第二步：在业务层使用@Transactional 注解

第三步：在配置文件中开启 spring 对注解事务的支持

# aop案例：

```java
package com.eagles.aop.account.dao;

public interface AccountDao {
    void test();

    void updateMoney();
}
```

```java
package com.eagles.aop.account.dao;

import com.eagles.aop.utils.JDBCUtils;
import org.springframework.stereotype.Repository;

import java.sql.PreparedStatement;
import java.sql.SQLException;
@Repository
public class AccountDaoImpl implements AccountDao{
    @Override
    public void test() {

    }
    PreparedStatement preparedStatement =null;

    /**
     * 转账的逻辑
     * A账户 - 钱
     * B账户 + 钱
     */
    @Override
    public void updateMoney() {
        try {
            preparedStatement = JDBCUtils.getConnection().prepareStatement("update tb_account set account_A =account_A - ? where id=?");
            preparedStatement.setInt(1,1000);
            preparedStatement.setInt(2,1);
            System.out.println(preparedStatement.executeUpdate() == 1? "A账户转账成功" :"A账户转账失败");

            int i =1 / 0;

            preparedStatement = JDBCUtils.getConnection().prepareStatement("update tb_account set account_B =account_B + ? where id=?");
            preparedStatement.setInt(1,1000);
            preparedStatement.setInt(2,1);
            System.out.println(preparedStatement.executeUpdate() == 1? "转账成功" :"转账失败");

        } catch (SQLException e) {
            throw new RuntimeException(e);
        }

    }
}
```

```java
package com.eagles.aop.account.service;

public interface AccountService {
    void test();

    void updateMoney();
}
```

```java
package com.eagles.aop.account.service;

import com.eagles.aop.account.dao.AccountDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class AccountServiceImpl implements AccountService{
    @Autowired
    private AccountDao accountDao;

    @Override
    public void test() {

    }

    @Override
    public void updateMoney() {
        accountDao.updateMoney();
    }
}
```

```java
package com.eagles.aop.proxy;

import com.eagles.aop.account.service.AccountService;
import com.eagles.aop.transaction.TransactionManager;
import org.springframework.stereotype.Component;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

@Component
public class AopProxy implements InvocationHandler {

    private AccountService accountService;

    public AopProxy(AccountService accountService) {
        this.accountService = accountService;
    }
    public  Object getProxy(){
        return Proxy.newProxyInstance(accountService.getClass().getClassLoader(),accountService.getClass().getInterfaces(),this );
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object invoke =null;
        try{
            //前置通知
            TransactionManager.beforeTransaction();

            //调用目标方法
            invoke = method.invoke(accountService, args);

            //后置通知
            TransactionManager.afterTransaction();

        }catch (Exception e){
            //异常通知
            TransactionManager.exceptionTransaction();
        }finally {
            //最终通知
            TransactionManager.endTransaction();
        }

        return invoke;
    }
}
```

```java
package com.eagles.aop.test;

import com.eagles.aop.account.service.AccountService;
import com.eagles.aop.proxy.AopProxy;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/beans.xml")
public class AopTest {

    @Autowired
    AccountService accountService;

    @Autowired
    AopProxy aopProxy;

    @Test
    public void test01(){
        accountService.updateMoney();
    }

    @Test
    public void Test02(){
        //代理对象
        AccountService service = (AccountService)aopProxy.getProxy();

        service.updateMoney();
    }
}
```

```java
package com.eagles.aop.transaction;

import com.eagles.aop.utils.JDBCUtils;

import java.sql.SQLException;

/**
 * 通知类 具体增强的内容
 */
public class TransactionManager {

    /*
    前置通知 开启事务
     */
    public static void beforeTransaction(){
        try {
            System.out.println("前置通知执行了");
            JDBCUtils.getConnection().setAutoCommit(false);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    /*
    后置通知 提交事务
     */
    public static void afterTransaction(){
        try {
            System.out.println("后置通知执行了");
            JDBCUtils.getConnection().commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    /*
    异常通知 回滚事务
     */
    public static  void exceptionTransaction(){
        try {
            System.out.println("异常通知执行了");
            JDBCUtils.getConnection().rollback();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    /*
    最终通知 回收资源
     */
    public static void endTransaction(){
        try {
            System.out.println("最终通知执行了");
            JDBCUtils.getConnection().close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
package com.eagles.aop.utils;
import java.sql.*;

public class JDBCUtils {

    public static final String DATABASE_NAME = "database1";
    public static final String USERNAME = "root";
    public static final String PASSWORD = "";
    static Connection connection =null;

    //构造方法私有化
    private JDBCUtils(){}

    static {

        //注册驱动
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        try {
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/" + DATABASE_NAME + "?characterEncoding=utf8&serverTimezone=GMT%2B8", USERNAME, PASSWORD);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 数据库连接对象
     * @return
     */
    public static Connection getConnection()  {

        return  connection;

    }

    /**
     * 回收数据库资源
     * @param resultSet
     * @param statement
     * @param connection
     */
    public static void close(ResultSet resultSet,Statement statement,Connection connection){

        if (resultSet != null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        if (statement != null){
            try {
                statement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        if (connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
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
  <!--包扫描-->
  <context:component-scan base-package="com.eagles"></context:component-scan>
</beans>
```

# xml方式：

```java
package com.eagles.aop.service;

import org.springframework.stereotype.Service;

@Service()
public class TestServiceImpl implements TestService {
    @Override
    public void getMethod01() {
        System.out.println("核心业务1");

        int i = 1/0;
    }

    @Override
    public void getMethod02() {
        System.out.println("核心业务2");
    }

    @Override
    public void getMethod03() {
        System.out.println("核心业务3");
    }

    @Override
    public void getMethod04() {
        System.out.println("核心业务4");
    }

    @Override
    public void getMethod05() {
        System.out.println("核心业务5");
    }

    @Override
    public void test01() {
        System.out.println("test01");
    }

    @Override
    public void test02() {
        System.out.println("test02");
    }
}
```

```java
package com.eagles.aop.service;

public interface TestService {
    void getMethod01();
    void getMethod02();
    void getMethod03();
    void getMethod04();
    void getMethod05();
    void test01();
    void test02();
}
```

```java
package com.eagles.aop.service;

import org.springframework.stereotype.Service;

@Service()
public class TestServiceImpl implements TestService {
    @Override
    public void getMethod01() {
        System.out.println("核心业务1");

        int i = 1/0;
    }

    @Override
    public void getMethod02() {
        System.out.println("核心业务2");
    }

    @Override
    public void getMethod03() {
        System.out.println("核心业务3");
    }

    @Override
    public void getMethod04() {
        System.out.println("核心业务4");
    }

    @Override
    public void getMethod05() {
        System.out.println("核心业务5");
    }

    @Override
    public void test01() {
        System.out.println("test01");
    }

    @Override
    public void test02() {
        System.out.println("test02");
    }
}
```

```java
package com.eagles.aop.test;
import com.eagles.aop.service.TestService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/aop.xml")
public class AopTest02 {

    @Autowired
    TestService testService;

    @Test
    public void test01(){
        testService.getMethod01();
    }
    @Test
    public void test02(){
        testService.getMethod02();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/aop
  http://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--将通知类的实例添加到容器中-->
  <bean id="loggerAdvice" class="com.eagles.aop.advice.LoggerAdvice"></bean>

  <!--将目标类的实例添加到容器中-->
  <bean id="testService" class="com.eagles.aop.service.TestServiceImpl"></bean>

  <!--配置切面-->
  <aop:config>
    <aop:aspect ref="loggerAdvice">
      <aop:pointcut id="pointcut01" expression="execution(* com.eagles.aop.service.TestService.getMethod*(..))"/>
      <!--配置前置通知-->
      <aop:before method="beforeAdvice" pointcut-ref="pointcut01"/>
      <!--配置后置通知-->
      <aop:after-returning method="afterAdvice" pointcut-ref="pointcut01"/>
      <!--配置异常通知-->
      <aop:after-throwing method="exceptionAdvice" pointcut-ref="pointcut01"/>
      <!--配置最终通知-->
      <aop:after method="endAdvice" pointcut-ref="pointcut01"/>
    </aop:aspect>
  </aop:config>

</beans>
```

```java
前置通知启动了
核心业务1
后置通知启动了
最终通知启动了
```

# 注解的方式：

```java
package com.eagles.aop.advice;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

/*
收集日志的通知类 具体增强的内容
@Aspect  切面类 该注释声明当前类为通知类 开始配置切面
@Component  组件类
 */
@Aspect
@Component
public class LoggerAdvice {
    /**
     * 声明切入点表达式
     */
    @Pointcut("execution(* com.eagles.aop.service.TestService.getMethod*(..))")
    public void pt01(){

    }

    /*
    前置通知
     */
    @Before("pt01()")
    public void beforeAdvice(){
        System.out.println("前置通知启动了");
    }
    /*
    后置通知
     */
    @AfterReturning("pt01()")
    public void afterAdvice(){
        System.out.println("后置通知启动了");
    }
    /*
    异常通知
     */
    @AfterThrowing("pt01()")
    public void exceptionAdvice(){
        System.out.println("异常通知启动了");
    }
    /*
    最终通知
     */
    @After("pt01()")
    public void endAdvice(){
        System.out.println("最终通知启动了");
    }

    //
    //    /**
    //     * 环绕通知
    //     */
    //    public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint){
    //        try {
    //            System.out.println("环绕通知前置");
    //
    //            Object proceed = proceedingJoinPoint.proceed();
    //
    //            //获取切入点的参数列表
    //            Object[] args = proceedingJoinPoint.getArgs();
    //            System.out.println("参数列表"+args[0]);
    //
    //            System.out.println("环绕通知后置");
    //        } catch (Throwable throwable) {
    //            throwable.printStackTrace();
    //            System.out.println("环绕通知异常");
    //        }finally {
    //            System.out.println("环绕通知最终");
    //        }
    //    }
}
```

前置通知+后置通知+异常通知+最终通知=环绕通知

```java
package com.eagles.aop.service;

public interface TestService {
    void getMethod01();
    void getMethod02();
    void getMethod03();
    void getMethod04();
    void getMethod05();
    void test01();
    void test02();

    void update1();
    void update2();
    void update3();

}
```

void update1();

void update2();

void update3();

是为了表示用update写增删改查的方法，到时候调用update\*就可以用这些方法

void getMethod03(int i); 是为了讲解环绕通知有方法可以获取参数的方法

```java
package com.eagles.aop.service;

import org.springframework.stereotype.Service;

@Service
public class TestServiceImpl implements TestService {
    @Override
    public void getMethod01() {
        System.out.println("核心业务1");

        int i = 1/0;
    }

    @Override
    public void getMethod02() {
        System.out.println("核心业务2");
    }

    @Override
    public void getMethod03() {
        System.out.println("核心业务3");
    }

    @Override
    public void getMethod04() {
        System.out.println("核心业务4");
    }

    @Override
    public void getMethod05() {
        System.out.println("核心业务5");
    }

    @Override
    public void test01() {
        System.out.println("test01");
    }

    @Override
    public void test02() {
        System.out.println("test02");
    }

    @Override
    public void update1() {

    }

    @Override
    public void update2() {

    }

    @Override
    public void update3() {

    }
}
```

```java
package com.eagles.aop.test;
import com.eagles.aop.service.TestService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/aop2.xml")
public class AopTest03 {

    @Autowired
    TestService testService;

    @Test
    public void test01(){

        testService.getMethod03();
    }

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/aop
  http://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--开启springAop对注解的支持-->
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
  <!--开启包扫描-->
  <context:component-scan base-package="com.eagles"></context:component-scan>

</beans>
```

环绕通知可以获取参数

```java
/**
 * 环绕通知
 */
@Around("pt01()")
public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint){
try {
    System.out.println("环绕通知前置");

    Object proceed = proceedingJoinPoint.proceed();

    //获取切入点的参数列表
    Object[] args = proceedingJoinPoint.getArgs();
    System.out.println("参数列表"+args[0]);

    System.out.println("环绕通知后置");
} catch (Throwable throwable) {
    throwable.printStackTrace();
    System.out.println("环绕通知异常");
}finally {
    System.out.println("环绕通知最终");
}
}
```

```java
package com.eagles.aop.service;

public interface TestService {

    void getMethod03(int i);

}
```

```java
package com.eagles.aop.service;

import org.springframework.stereotype.Service;

@Service
public class TestServiceImpl implements TestService {

    @Override
    public void getMethod03(int i) {
        System.out.println("核心业务3");
    }

}
```

```java
package com.eagles.aop.test;
import com.eagles.aop.service.TestService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:xml/aop2.xml")
public class AopTest03 {

    @Autowired
    TestService testService;

    @Test
    public void test01(){

        testService.getMethod03(100);
    }

}
```

```plain
环绕通知前置
核心业务3
参数列表100
环绕通知后置
环绕通知最终
```

# aop事务控制基于xml：

```java
package com.eagles.tx.account.dao;

public interface AccountDao {
    void test();

    void updateMoney();

    void updateAccount();

    void updateAccountById();

    void findAll();
}
```

```java
package com.eagles.tx.account.dao;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class AccountDaoImpl implements AccountDao {

    private JdbcTemplate jdbcTemplate;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public void test() {

    }

    /**
     * 转账的逻辑
     * A账户 - 钱
     * B账户 + 钱
     */
    @Override
    public void updateMoney() {

        int update = jdbcTemplate.update("update tb_account set account_A =account_A - ? where id=?", 1000, 1);
        System.out.println(update == 1 ? "A账户转账成功" : "A账户转账失败");

        //int i =1 / 0;

        update = jdbcTemplate.update("update tb_account set account_B =account_B + ? where id=?", 1000, 1);
        System.out.println(update == 1 ? "转账成功" : "转账失败");

    }

    @Override
    public void updateAccount() {

    }

    @Override
    public void updateAccountById() {

    }

    @Override
    public void findAll() {

    }

}
```

int i =1 / 0; 即使程序运行时报错，数据库的数据也不会改变，因为spring底层默认回滚数据

```java
package com.eagles.tx.account.service;

public interface AccountService {
    void test();

    void updateMoney();

    void updateAccount();

    void updateAccountById();

    void fidnAll();
}
```

```java
package com.eagles.tx.account.service;

import com.eagles.tx.account.dao.AccountDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class AccountServiceImpl implements AccountService {
    @Autowired
    private AccountDao accountDao;

    @Override
    public void test() {
        accountDao.test();
    }

    @Override
    public void updateMoney() {

        accountDao.updateMoney();
    }

    @Override
    public void updateAccount() {
        accountDao.updateAccount();
    }

    @Override
    public void updateAccountById() {
        accountDao.updateAccountById();
    }

    @Override
    public void fidnAll() {

    }

}
```

```java
package com.eagles.tx.account.text;

import com.eagles.tx.account.service.AccountService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@ContextConfiguration(locations = "classpath:xml/tx.xml")
@RunWith(SpringJUnit4ClassRunner.class)
public class TxTest {
    @Autowired
    AccountService accountService;

    @Test
    public void test(){
        accountService.updateMoney();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="
  http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/tx
  http://www.springframework.org/schema/tx/spring-tx.xsd
  http://www.springframework.org/schema/aop
  http://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--将持久层的实例添加到容器中-->
  <bean id="dao" class="com.eagles.tx.account.dao.AccountDaoImpl">
    <!--注入jdbcTemplate-->
    <property name="jdbcTemplate" ref="jdbcTemplate"></property>
  </bean>

  <!--注入业务层的实例（目标对象）-->
  <bean id="service" class="com.eagles.tx.account.service.AccountServiceImpl"></bean>

  <!--装配jdbcTemplate的实例-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入数据源的实例-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>

  <!--装配数据源的实例-->
  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <!--注入连接数据库相关的参数-->
    <property name="url" value="jdbc:mysql://localhost:3306/database1?characterEncoding=utf8&amp;serverTimezone=GMT%2B8"></property>
    <property name="username" value="root"></property>
    <property name="password" value=""></property>
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
  </bean>
  <!--配置事务管理器对象-->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源的实例-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--配置事务的通知-->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!--事务的配置项-->

    <!--
    propagation: 是否开启事务
    -REQUIRED 如果当前的业务没有事务支持，就开启事务 默认值
    -SUPPORTS 如果当前的业务没有事务支持，则不会开启新的事务

    read-only ：是否开启只读
    -如果是查询的场景 需要开启只读 设置为true
    -如果是增删改的场景 需要关闭只读 设置为false
    timeout 事务的超时时间 默认值是-1 表示永不超时
    -设置的参数如果是正数，那么就以s为单位，指定时间

    isolation 事务的隔离级别
    -这个取默认值 DEFAULT 默认就是数据库级别的隔离

    rollback-for 指定一个异常，除了该异常回滚之外，其他异常都不回滚
    no-rollback-for 指定一个异常，除了该异常不回滚之外，其他异常都回滚
    -默认情况 所有异常都回滚

    -->

    <tx:attributes>
      <tx:method name="update*" propagation="REQUIRED" read-only="false"/>
      <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
    </tx:attributes>
  </tx:advice>

  <!--设置切面-->
  <aop:config>
    <!--需要增强哪些方法-->
    <aop:pointcut id="pt01" expression="execution(* com.eagles.tx.account.service.*.*(..))"/>
    <!--设置切入点表达式和对应的事务通知-->
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt01"></aop:advisor>
  </aop:config>

</beans>
```

# aop事务控制基于注解：

```java
package com.eagles.tx.account.dao;

public interface AopDao {
    void test();

    void updateMoney();

    void updateAccount();

    void updateAccountById();

    void findAll();
}
```

```java
package com.eagles.tx.account.dao;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class AopDaoImpl implements AopDao {

    private JdbcTemplate jdbcTemplate;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public void test() {

    }

    /**
     * 转账的逻辑
     * A账户 - 钱
     * B账户 + 钱
     */
    @Override
    public void updateMoney() {

        int update = jdbcTemplate.update("update tb_account set account_A =account_A - ? where id=?", 1000, 1);
        System.out.println(update == 1 ? "A账户转账成功" : "A账户转账失败");

        //int i =1 / 0;

        update = jdbcTemplate.update("update tb_account set account_B =account_B + ? where id=?", 1000, 1);
        System.out.println(update == 1 ? "转账成功" : "转账失败");

    }

    @Override
    public void updateAccount() {

    }

    @Override
    public void updateAccountById() {

    }

    @Override
    public void findAll() {

    }

}
```

```java
package com.eagles.tx.account.service;

public interface AopService {
    void test();

    void updateMoney();

    void updateAccount();

    void updateAccountById();

    void fidnAll();
}
```

```java
package com.eagles.tx.account.service;

import com.eagles.tx.account.dao.AopDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import static org.springframework.transaction.annotation.Propagation.REQUIRED;
import static org.springframework.transaction.annotation.Propagation.SUPPORTS;

@Service
@Transactional(propagation = REQUIRED ,readOnly  = false)
public class AopServiceImpl implements AopService {
    @Autowired
    private AopDao aopDao;

    @Override
    public void test() {
        aopDao.test();
    }

    @Override
    @Transactional(propagation = REQUIRED ,readOnly  = false)
    public void updateMoney() {

        aopDao.updateMoney();
    }

    @Override
    @Transactional(propagation = REQUIRED ,readOnly  = false)

    public void updateAccount() {
        aopDao.updateAccount();
    }

    @Override
    @Transactional(propagation = REQUIRED ,readOnly  = false)
    public void updateAccountById() {

        aopDao.updateAccountById();
    }

    @Override
    @Transactional(propagation = SUPPORTS ,readOnly  = true)
    public void fidnAll() {
        aopDao.findAll();
    }

}
```

```java
package com.eagles.tx.account.text;

import com.eagles.tx.account.service.AopService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@ContextConfiguration(locations = "classpath:xml/tx2.xml")
@RunWith(SpringJUnit4ClassRunner.class)
public class TxTest2 {
    @Autowired
    AopService aopService;

    @Test
    public void test(){
        aopService.updateMoney();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="
  http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/tx
  http://www.springframework.org/schema/tx/spring-tx.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/aop
  http://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--开启spring对注解的支持-->
  <tx:annotation-driven></tx:annotation-driven>
  <!--开启包扫描-->
  <context:component-scan base-package="com.eagles"></context:component-scan>

  <!--将持久层的实例添加到容器中-->
  <bean id="aopDao" class="com.eagles.tx.account.dao.AopDaoImpl">
    <!--注入jdbcTemplate-->
    <property name="jdbcTemplate" ref="jdbcTemplate"></property>
  </bean>

  <!--装配jdbcTemplate的实例-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入数据源的实例-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>

  <!--装配数据源的实例-->
  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <!--注入连接数据库相关的参数-->
    <property name="url" value="jdbc:mysql://localhost:3306/database1?characterEncoding=utf8&amp;serverTimezone=GMT%2B8"></property>
    <property name="username" value="root"></property>
    <property name="password" value=""></property>
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
  </bean>

  <!--配置事务管理器对象-->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源实例-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>

  <!--配置事务的通知-->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!--事务的配置项-->
    <!--
    propagation: 是否开启事务
    -REQUIRED 如果当前的业务没有事务支持，就开启事务 默认值
    -SUPPORTS 如果当前的业务没有事务支持，则不会开启新的事务

    read-only ：是否开启只读
    -如果是查询的场景 需要开启只读 设置为true
    -如果是增删改的场景 需要关闭只读 设置为false
    timeout 事务的超时时间 默认值是-1 表示永不超时
    -设置的参数如果是正数，那么就以s为单位，指定时间

    isolation 事务的隔离级别
    -这个取默认值 DEFAULT 默认就是数据库级别的隔离

    rollback-for 指定一个异常，除了该异常回滚之外，其他异常都不回滚
    no-rollback-for 指定一个异常，除了该异常不回滚之外，其他异常都回滚
    -默认情况 所有异常都回滚

    -->
    <tx:attributes>
      <!--业务场景如果是增删改的话-->
      <tx:method name="update*" propagation="REQUIRED" read-only="false"/>
      <!--如果查询相关的场景-->
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>

        </tx:attributes>
    </tx:advice>

    <!--配置切面-->
    <aop:config>
        <!--需要增强哪些方法-->
        <aop:pointcut id="pt01" expression="execution(* com.eagles.tx.account.service.*.*(..))"/>
        <!--配置切入点表达式和对应的事务通知-->
        <aop:advisor advice-ref="txAdvice"  pointcut-ref="pt01"></aop:advisor>
    </aop:config>

</beans>
```
