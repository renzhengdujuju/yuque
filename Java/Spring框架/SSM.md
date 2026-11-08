---
title: "SSM"
yuque_slug: "qip7ufsod3eftpcq"
yuque_id: 232696586
yuque_updated: "2026-10-14T04:54:33.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/qip7ufsod3eftpcq"
---
## 什么是 SSM？

**SSM** 是三个主流 Java Web 开发框架的首字母缩写，分别是：

- **S**：**Spring**
- **S**：**Spring MVC**
- **M**：**MyBatis**

它们组合在一起，构成了一个轻量级、高效、灵活的 Java Web 全栈开发框架组合，广泛应用于企业级后台系统开发（尤其是传统单体架构项目）。

---

## 一、SSM 三大组件详解

### 1. **MyBatis**（持久层框架）

#### 作用：

- 负责与数据库交互，替代传统的 JDBC。
- 将 SQL 语句与 Java 代码解耦，通过 XML 或注解方式配置 SQL。
- 自动完成数据库记录与 Java 对象之间的映射（ORM：对象关系映射）。

#### 核心组件：

- `SqlSessionFactory`：创建数据库会话工厂。
- `SqlSession`：执行 SQL 的会话对象。
- `Mapper` 接口 + XML 配置文件：定义 SQL 和结果映射。

#### 示例：

```plain
java

深色版本

// UserMapper.java
public interface UserMapper {
    User findById(int id);
}
```

```plain
xml

深色版本

<!-- UserMapper.xml -->
<select id="findById" resultType="User">
    SELECT * FROM user WHERE id = #{id}
</select>
```

MyBatis 让你“写 SQL 更自由”，同时避免了大量重复的 JDBC 代码。

---

### 2. **Spring MVC**（表现层/控制层框架）

#### 作用：

- 处理 HTTP 请求和响应（如页面跳转、接收参数、返回数据）。
- 基于 MVC 设计模式（Model-View-Controller），实现前后端分离（逻辑上）。
- 是 Spring 框架的一部分，专用于 Web 层。

#### 核心组件：

- `@Controller`：标识控制器类。
- `@RequestMapping`：映射 URL 到具体方法。
- `Model/ModelAndView`：传递数据给视图。
- `@ResponseBody`：直接返回 JSON 数据（用于前后端分离）。
- `ViewResolver`：解析视图名（如 JSP、FreeMarker）。

#### 示例：

```plain
java

深色版本

@Controller
public class UserController {
    @RequestMapping("/user")
    public String getUser(Model model) {
        User user = userService.findUser(1);
        model.addAttribute("user", user);
        return "user"; // 返回视图名（如 user.jsp）
    }
}
```

---

### 3. **Spring**（核心容器框架）

#### 作用：

- 整个 SSM 的“粘合剂”和“管理中枢”。
- 提供 **IoC（控制反转）** 和 **AOP（面向切面编程）** 功能。
- 管理所有 Java 对象（Bean）的生命周期和依赖关系（无需手动 `new`）。
- 整合 MyBatis 和 Spring MVC。

#### 核心概念：

- **IoC 容器**：Spring 启动时自动创建并管理所有 Bean。
- **DI（依赖注入）**：通过 `@Autowired` 自动注入依赖对象。
- **AOP**：实现日志、事务、权限等横切关注点的统一处理。

#### 示例：

```plain
java

深色版本

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper; // 自动注入，无需 new

    public User findUser(int id) {
        return userMapper.findById(id);
    }
}
```

---

## 二、SSM 的工作流程（请求处理流程）

```plain
深色版本

浏览器 → [DispatcherServlet] → [HandlerMapping] → [Controller]
                             ↓
                     [调用 Service] → [Spring 管理的业务逻辑]
                             ↓
                     [调用 Mapper] → [MyBatis 操作数据库]
                             ↓
                ← 返回数据/视图名 ←
                             ↓
                 [ViewResolver] → [渲染页面（JSP/FTL）]
                             ↓
                        返回给浏览器
```

### 具体步骤：

1. 用户发送请求（如 `/user/list`）。
2. Spring MVC 的前端控制器 `DispatcherServlet` 接收请求。
3. 通过 `HandlerMapping` 找到对应的 `@Controller` 方法。
4. 调用 `@Service` 业务层方法（由 Spring 管理）。
5. 业务层调用 `@Mapper` 接口操作数据库（MyBatis 实现）。
6. 数据返回给 Controller，封装成 `Model` 或 JSON。
7. 视图解析器渲染页面（如 JSP、FreeMarker）或直接返回 JSON。
8. 最终响应返回浏览器。

---

## 三、SSM 项目的典型结构

```plain
深色版本

src/
├── main/
│   ├── java/
│   │   └── com/example/demo/
│   │       ├── controller/    → Spring MVC 控制器
│   │       ├── service/       → Spring 业务逻辑
│   │       ├── mapper/        → MyBatis 接口
│   │       └── pojo/          → 实体类（User, Order 等）
│   ├── resources/
│   │   ├── mapper/            → MyBatis XML 映射文件
│   │   ├── application.yml    → Spring 配置文件
│   │   └── static/            → 静态资源（CSS, JS）
│   │   └── templates/         → 模板文件（JSP, FTL）
```

---

## 四、SSM 的优缺点

|  |  |
| --- | --- |
| **优点** | **缺点** |
| 结构清晰，分层明确（MVC） | 配置繁琐（XML 多，Spring + MyBatis 集成复杂） |
| 灵活强大，SQL 可控性强（MyBatis） | 学习成本较高（需掌握三个框架） |
| 社区成熟，资料丰富 | 不如 Spring Boot 快速开发 |
| 适合中大型企业项目 | 微服务时代逐渐被 Spring Boot + MyBatis 取代 |

---

## 五、SSM vs Spring Boot

|  |  |  |
| --- | --- | --- |
| **对比项** | **SSM** | **Spring Boot（+ MyBatis）** |
| 配置方式 | 手动 XML/Java 配置较多 | 约定优于配置，自动装配，极简配置 |
| 开发效率 | 较低 | 高（内置 Tomcat，快速启动） |
| 依赖管理 | 手动管理 Maven 依赖 | Starter 自动管理依赖 |
| 当前趋势 | 传统项目、老系统维护 | 新项目主流选择，支持微服务、云原生 |

**建议**：如果你是新手，建议从 **Spring Boot + MyBatis** 入手，它本质上是 SSM 的“现代化升级版”。

---

## 总结

|  |  |
| --- | --- |
| **框架** | **职责** |
| **MyBatis** | 操作数据库，实现 ORM |
| **Spring MVC** | 处理 Web 请求，返回页面或数据 |
| **Spring** | 管理对象、依赖注入、事务、整合全家桶 |

**一句话总结 SSM**：  
**Spring** 管对象，**Spring MVC** 管请求，**MyBatis** 管数据库，三者结合打造 Java Web 应用！

---

# 列表展示页面和分页的一个操作案例：

SSM商品管理系统项目笔记

一、项目概述

本项目是基于SSM（Spring + Spring MVC + MyBatis）框架开发的商品管理系统，主要功能包括：

商品列表展示（支持分页）

商品添加（含品牌选择、分类选择）

商品库存管理（重复添加商品时自动增加库存）

品牌数据加载（下拉框动态渲染）

二、项目结构

```plain
ssm-good/
├── src/
│   ├── main/
│   │   ├── java/com/eagles/ssm/
│   │   │   ├── controller/      // 控制器层（处理请求）
│   │   │   ├── service/         // 业务逻辑层接口
│   │   │   │   └── impl/        // 业务逻辑层实现
│   │   │   ├── mapper/          // 数据访问层接口
│   │   │   └── pojo/            // 实体类
│   │   └── resources/           // 配置文件
│   │       ├── com/eagles/ssm/mapper/  // MyBatis映射文件
│   │       ├── beans.xml        // Spring核心配置
│   │       ├── springmvc.xml    // Spring MVC配置
│   │       ├── db.properties    // 数据库连接配置
│   │       └── log4j.properties // 日志配置
│   └── test/                    // 测试目录（未展示）
├── web/                         // Web资源
│   ├── WEB-INF/
│   │   └── web.xml              // Web应用配置
│   ├── jsp/                     // JSP视图
│   │   ├── goods-list.jsp       // 商品列表页
│   │   └── goods-add.jsp        // 商品添加页
│   ├── jquery/                  // 静态资源（jQuery）
│   └── index.jsp                // 首页（未使用）
├── pom.xml                      // Maven依赖配置
└── 笔记                         // 项目笔记文件
```

---

三、核心组件说明

1. 实体类（POJO）

```plain
public class Brand {
    private int bid;
    private String brandName;

    // 无参构造（MyBatis反射需要）
    public Brand() {}
    
    // 有参构造
    public Brand(int bid, String brandName) {
        this.bid = bid;
        this.brandName = brandName;
    }

    // Getter/Setter（属性封装，MyBatis映射依赖）
    public String getBrandName() {
        return brandName;
    }
    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }

    // toString（调试用，打印对象信息）
    @Override
    public String toString() {
        return "Brand{" +
                "bid=" + bid +
                ", brandName='" + brandName + '\'' +
                '}';
    }
}
```

代码说明：

无参构造是MyBatis通过反射创建对象的必要条件；

Getter/Setter方法用于MyBatis结果集映射（resultMap中property属性对应）；

toString()便于调试时打印品牌信息。

---

Brand.java：品牌实体 属性：bid（品牌ID）、brandName（品牌名称），含构造方法、getter/setter、toString。

Cate.java：分类实体 属性：cid（分类ID）、cateName（分类名称），结构同Brand。

Goods.java：商品实体（核心） 属性：gid（商品ID）、goodsName（商品名称）、price（价格）、upTime（上架时间）、brandId（品牌ID）、cateId（分类ID）、goodsDesc（描述）、num（库存），关联Brand和Cate对象。

---

2. 数据访问层（Mapper）

BrandMapper.xml（品牌数据访问映射）

核心代码片段：

```plain
<mapper namespace="com.eagles.ssm.mapper.BrandMapper">
    <resultMap id="brandVo" type="com.eagles.ssm.pojo.Brand">
        <id column="bid" property="bid"/> <!-- 主键映射 -->
        <result column="brand_name" property="brandName"/> <!-- 字段映射：数据库列名→实体属性名 -->
    </resultMap>

    <select id="findBrands" resultMap="brandVo">
        select * from ssm_brand <!-- 查询所有品牌 -->
    </select>
</mapper>
```

代码说明：

namespace绑定BrandMapper.java接口，确保MyBatis能找到对应的方法；

resultMap解决数据库字段名（如brand\_name）与实体属性名（brandName）不一致问题；

select标签的id需与接口方法名完全一致（findBrands），MyBatis通过此关联SQL与接口。

---

BrandMapper.java：品牌数据访问接口 方法：List<Brand> findBrands()（查询所有品牌）。

GoodsMapper.java：商品数据访问接口 核心方法：

List<Goods> findGoodsAll(HashMap)（分页查询商品列表）

Goods findGoodsByName(String)（按名称查询商品是否存在）

int updateGoodsNum(int)（更新商品库存）

int insertGoods(Goods)（新增商品）。

MyBatis映射文件：

BrandMapper.xml：实现findBrands()，查询ssm\_brand表。

GoodsMapper.xml：实现商品CRUD，含分页查询（limit #{indexStart},#{pageSize}）、库存更新、新增等SQL。

---

3. 业务逻辑层（Service）

BrandServiceImpl.java（品牌服务实现类）

核心代码片段：

```plain
@Service
public class BrandServiceImpl implements BrandService {
    @Autowired
    private BrandMapper bandMapper; // ❗代码错误：变量名拼写错误，应为brandMapper

    @Override
    public List<Brand> findBrands() {
        return bandMapper.findBrands(); // 调用Mapper层查询品牌列表
    }
}
```

代码说明：

@Service注解标记该类为Spring管理的业务层组件，自动注入到IOC容器；

@Autowired自动装配BrandMapper接口的代理对象（MyBatis通过MapperScannerConfigurer生成）；

关键错误：bandMapper拼写错误（正确应为brandMapper），会导致Spring无法找到对应的Bean，运行时抛出NoSuchBeanDefinitionException。

---

BrandService.java：品牌服务接口，定义findBrands()。 BrandServiceImpl.java：实现类，通过BrandMapper查询品牌列表（注意：代码中存在拼写错误bandMapper，应为brandMapper，需修正）。

GoodsService.java：商品服务接口，定义列表查询、计数、新增方法。 GoodsServiceImpl.java：核心业务逻辑实现：

新增商品前检查名称是否存在（findGoodsByName）；

若存在：调用updateGoodsNum增加库存，返回"1"；

若不存在：设置默认库存（1）和当前上架时间，调用insertGoods新增，返回"2"；

异常情况返回"3"（失败）。

---

4. 控制层（Controller）与前端交互

品牌下拉框动态加载（goods-add.jsp）

核心代码片段：

```plain
<script>
    $(function (){
        $.ajax({
            url:"<%=path%>/brand/findBrands", // 请求后端接口
            type:"post",
            success:function (obj){ // obj为后端返回的List<Brand> JSON数组
                for (let i in obj){
                    // 动态生成<option>标签，value为品牌ID，文本为品牌名称
                    $("select").append("<option value='"+obj[i].bid+"'>"+obj[i].brandName+"</option>")
                }
            },
            dataType:"json" // 指定响应数据格式为JSON
        })
    })
</script>
```

代码说明：

页面加载完成后（$(function(){})），通过AJAX请求/brand/findBrands接口；

后端BrandController的findBrands()方法返回品牌列表JSON（@ResponseBody注解自动转换）；

前端遍历JSON数组，为<select>标签动态添加品牌选项，实现前后端数据交互。

---

BrandController.java：处理品牌相关请求

@RequestMapping("/brand/findBrands")：返回所有品牌JSON数据（供前端下拉框渲染）。

GoodsController.java：核心控制器

@RequestMapping("/goods/list")：处理商品列表请求，实现分页逻辑（计算prevPage、nextPage、lastPage），将商品数据和分页参数存入Model，跳转至goods-list.jsp。

@RequestMapping("/goods/toAdd")：跳转至商品添加页（goods-add.jsp）。

@RequestMapping("/goods/insertGoods")：接收添加商品请求，调用goodsService.insertGoods，返回结果给前端。

---

5. 前端页面（JSP）

goods-list.jsp：商品列表页

```plain
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--引入jstl标签库--%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%

    String path = request.getContextPath();

%>
<!DOCTYPE html>
<html lang="zh-CN">
</head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>商品管理系统</title>
    <link rel="stylesheet" href="css/goods-list.css">
</head>
<style>
    /* 基础样式重置 */
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Arial', sans-serif;
    }

    body {
        background-color: #cbd7e3;
        padding: 20px;
    }

    .container {
        max-width: 1200px;
        margin: 0 auto;
        background-color: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 0 10px rgba(110, 113, 117, 0.22);
    }

    /* 标题样式 */
    header h1 {
        text-align: center;
        color: #333;
        margin-bottom: 30px;
        padding: 10px 0;
        border-bottom: 2px solid #007bff;
    }

    /* 搜索栏样式 */
    .search-bar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
        padding: 0 10px;
    }

    .search-group {
        display: flex;
        gap: 10px;
    }

    #searchInput {
        padding: 8px 12px;
        border: 1px solid #ddd;
        border-radius: 4px;
        width: 300px;
        font-size: 14px;
    }

    .action-buttons {
        display: flex;
        gap: 10px;
    }

    .btn {
        padding: 8px 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;
        transition: background-color 0.3s;
    }

    .btn.primary {
        background-color: #007bff;
        color: white;
    }

    .btn.primary:hover {
        background-color: #0056b3;
    }

    .btn.secondary {
        background-color: #6c757d;
        color: white;
    }

    .btn.secondary:hover {
        background-color: #5a6268;
    }

    /* 表格样式 */
    .table-container {
        overflow-x: auto; /* 处理表格横向滚动 */
    }

    #goodsTable {
        width: 100%;
        border-collapse: collapse;
        text-align: left;
    }

    #goodsTable th,
    #goodsTable td {
        padding: 12px 15px;
        border: 1px solid #ddd;
    }

    #goodsTable th {
        background-color: #f8f9fa;
        font-weight: bold;
        color: #333;
    }

    #goodsTable tbody tr:nth-child(even) {
        background-color: #f2f2f2;
    }

    #goodsTable tbody tr:hover {
        background-color: #e9ecef;
    }

    /* 操作按钮样式 */
    .operate-btn {
        display: flex;
        gap: 5px;
    }

    .edit-btn {
        background-color: #28a745;
        color: white;
        padding: 5px 10px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
        font-size: 12px;
    }

    .edit-btn:hover {
        background-color: #218838;
    }

    .delete-btn {
        background-color: #dc3545;
        color: white;
        padding: 5px 10px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
        font-size: 12px;
    }

    .delete-btn:hover {
        background-color: #c82333;
    }
</style>
<%--引入jquery的控件--%>
<script src="<%=path%>/jquery/jquery-1.7.2.js"></script>
<script>
    function addGoods(){
        location.href="http://localhost:8087/ssm/goods/toAdd"
    }
    function page(currentPage){
        location.href="http://localhost:8087/ssm/goods/list?currentPage="+currentPage
    }

</script>

<body>
<div class="container">
    <!-- 标题区域 -->
    <header>
        <h1>商品列表</h1>
    </header>
    <!-- 搜索与操作区域 -->
    <div class="search-bar">
        <div class="search-group">
            <input type="text" id="searchInput" placeholder="请输入商品名称搜索...">
            <button onclick="searchGoods()">搜索</button>
        </div>
        <div class="action-buttons">
            <button id="addBtn" class="btn primary" onclick="addGoods()">添加商品</button>
            <button class="btn btn-deleteAll">批量删除</button>
        </div>
    </div>

    <!-- 商品列表表格 -->
    <div class="table-container">
        <table id="goodsTable">
            <thead>
            <tr>
                <th>商品ID</th>
                <th>商品名称</th>
                <th>价格</th>
                <th>上架时间</th>
                <th>所属品牌</th>
                <th>所属分类</th>
                <th>商品描述</th>
                <th>库存数量</th>
                <th>操作</th>
            </tr>
            </thead>
            <tbody >
            <c:forEach items="${goodsList}" var="goods">
                <tr>
                    <td>${goods.gid}</td>
                    <td>${goods.goodsName}</td>
                    <td>${goods.price}</td>
                    <td>${goods.upTime}</td>
                    <td>${goods.brand.brandName}</td>
                    <td>${goods.cate.cateName}</td>
                    <td>${goods.goodsDesc}</td>
                    <td>${goods.num}</td>

                    <td>
                        <button class="btn btn-edit" onclick="window.location.href='<%=path%>/goods/edit?gid=${goods.gid}'">编辑</button>
                        <button class="btn btn-delete" onclick="if(confirm('确定要删除该商品吗？')) window.location.href='<%=path%>/goods/delete?gid=${goods.gid}'">删除</button>
                    </td>

                </tr>
            </c:forEach>
            </tbody>
            <tr>
                <td colspan="100" align="center">
                    当前页${currentPage} / ${lastPage}
                    <a href="#" onclick="page(1)">首页</a>
                    <a href="#" onclick="page(${prevPage})">上一页</a>
                    <a href="#" onclick="page(${nextPage})">下一页</a>
                    <a href="#" onclick="page(${lastPage})">尾页</a>
                </td>
            </tr>
        </table>
    </div>
</div>

<script src="js/goods-list.js"></script>
</body>
</html>
```

展示商品表格（ID、名称、价格、品牌、分类等）；

分页控件（首页、上一页、下一页、尾页）；

"添加商品"按钮跳转至添加页。

goods-add.jsp：商品添加页

```plain
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--引入jstl的标签库--%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
    String path = request.getContextPath();
%>
<html>
<head>
    <title>商品上架</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f9;
        }
        .container {
            background-color: #fff;
            max-width: 600px;
            margin: 20px auto;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .form-title {
            text-align: center;
            color: #007BFF;
            margin-bottom: 30px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: bold;
        }
        .form-group input[type="text"],
        .form-group input[type="number"],
        .form-group textarea,
        .form-group select { /* 为下拉列表添加样式 */
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .form-group textarea {
            height: 80px;
            resize: vertical;
        }
        .btn-group {
            margin-top: 30px;
            text-align: center;
        }
        .btn-submit {
            background-color: #28a745;
            color: white;
            padding: 12px 25px;
            margin-right: 15px;
        }
        .btn-back {
            background-color: #6c757d;
            color: white;
            padding: 12px 25px;
        }
        .btn-submit:hover {
            background-color: #218838;
        }
        .btn-back:hover {
            background-color: #5a6268;
        }
        .category-checkbox-group { /* 多选框组样式 */
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .category-checkbox-group label { /* 多选框标签样式 */
            display: flex;
            align-items: center;
            gap: 6px;
            font-weight: normal;
            color: #333;
        }
    </style>
</head>
<script src="<%=path%>/js/jquery-1.7.2.js"></script>
<script>

    $(function (){
        $.ajax({
            url:"<%=path%>/brand/findBrands",
            type:"post",
            success:function (obj){
                for (let i in obj){
                    $("select").append("<option value='"+obj[i].bid+"'>"+obj[i].brandName+"</option>")
                }
            },
            dataType:"json"
        })

        $("#add").click(function (){
            $.ajax({
                url:"<%=path%>/goods/insertGoods",
                type:"post",
                data:$("form").serialize(),
                success:function (obj){
                    if (obj == "1"){
                        alert("您上架的商品已经存在，为您添加了库存！")
                        location.href="<%=path%>/goods/list"
                    }else if(obj == "2"){
                        alert("商品上架成功~")
                        location.href="<%=path%>/goods/list"
                    }else {
                        alert("商品上架失败，有问题请联系管理员~")
                    }
                },
                dataType:"text"
            })
        })

    })

</script>
<body>
<div class="container">
    <h2 class="form-title">商品上架</h2>
    <form>
        <div class="form-group">
            <label>商品名称：</label>
            <input type="text" name="goodsName" required>
        </div>

        <div class="form-group">
            <label>商品价格：</label>
            <input type="number" step="0.01" name="price" required>
        </div>

        <div class="form-group">
            <label>品牌：</label>
            <select name="brandId">
                <option value="">--请选择--</option>
            </select>
        </div>

        <div class="form-group">
            <label>分类：</label>
            <div class="category-checkbox-group">
                <label>
                    <input type="radio" name="cateId" value="1"> 家用电器
                </label>
                <label>
                    <input type="radio" name="cateId" value="3"> 运动器材
                </label>
                <label>
                    <input type="radio" name="cateId" value="4"> 服装服饰
                </label>
                <label>
                    <input type="radio" name="cateId" value="2"> 电子产品
                </label>
            </div>
        </div>

        <div class="form-group">
            <label>商品描述：</label>
            <textarea name="goodsDesc"></textarea>
        </div>

        <div class="btn-group">
            <button type="button" id="add" class="btn-submit">提交上架</button>
            <button type="button" class="btn-back" onclick="history.back()">返回列表</button>
        </div>
    </form>
</div>
</body>
</html>
```

表单含商品名称、价格、品牌下拉框（通过AJAX加载品牌数据）、分类单选框（静态选项：家用电器、运动器材等）；

"提交上架"按钮通过AJAX提交表单，根据返回结果（"1"/"2"/"3"）显示不同提示，并跳转至列表页。

---

6. 配置文件

web.xml：配置DispatcherServlet、字符编码过滤器（UTF-8）、Spring上下文监听器。

```plain
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--dispatcherServlet-->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--绑定springmvc的核心配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--设置启动级别-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--绑定一个全局的核心容器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:beans.xml</param-value>
    </context-param>

    <!--注册一个监听器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--拦截客户端的请求-->
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--将所有资源的字符集设置为utf-8-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>

    <!--拦截所有资源，同意设置字符集-->
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

beans.xml（Spring配置）：数据源（dbcp）、SqlSessionFactory、Mapper扫描器（扫描com.eagles.ssm.mapper）。

```plain
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启扫描包-->
    <context:component-scan base-package="com.eagles.ssm"></context:component-scan>

    <!--读取外部配置文件-->
    <context:property-placeholder location="classpath:db.properties"></context:property-placeholder>

    <!--注册数据源实例-->
    <bean id="basicDataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <!--注入源数据-->
        <property name="url" value="${db.url}"></property>
        <property name="driverClassName" value="${db.driver}"></property>
        <property name="username" value="${db.username}"></property>
        <property name="password" value="${db.password}"></property>
    </bean>

    <!--注册mybatis的工厂对象-->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--注入数据源对象-->
        <property name="dataSource" ref="basicDataSource"></property>
        <!--指定映射文件的位置-->
        <property name="mapperLocations" value="classpath:com/eagles/ssm/mapper/*.xml"></property>
    </bean>

    <!--mybatis的扫描器-->
    <!--
        扫描持久层包下所有的接口，并且通过反射创建对应的实现类，将代理出来的实现类装配到spring的核心容器中
    -->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--指定持久层包的位置-->
        <property name="basePackage" value="com.eagles.ssm.mapper"></property>
    </bean>

</beans>
```

springmvc.xml（Spring MVC配置）：组件扫描、注解驱动、视图解析器（前缀/jsp/，后缀.jsp）。

```plain
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--开启扫描包-->
    <context:component-scan base-package="com.eagles.ssm"></context:component-scan>

    <!--开启mvc对注解的支持-->
    <mvc:annotation-driven></mvc:annotation-driven>

    <!--过滤静态资源-->
    <mvc:default-servlet-handler></mvc:default-servlet-handler>

    <!--配置视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/jsp/"></property>
        <!--后缀-->
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```

db.properties：数据库连接参数（MySQL驱动、URL、用户名、密码）。

```plain
db.driver=com.mysql.cj.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/database1?serverTimezone=UTC&characterEncoding=utf-8
db.username=root
db.password=
```

没密码所以为空

---

四、核心业务流程

1. 商品列表展示流程

用户访问/goods/list → GoodsController.findGoodsAll处理请求；

计算分页参数（当前页、起始索引、总页数），调用goodsService.findGoodsAll查询商品列表；

将商品数据和分页参数存入Model，转发至goods-list.jsp；

JSP通过<c:forEach>遍历goodsList，渲染表格和分页控件。

2. 商品添加流程

用户点击"添加商品" → 跳转至goods-add.jsp；

页面加载时通过AJAX请求/brand/findBrands，获取品牌数据并渲染下拉框；

用户填写表单并提交 → AJAX请求/goods/insertGoods，发送表单数据；

GoodsController.insertGoods调用goodsService.insertGoods：

若商品已存在 → 库存+1，返回"1"；

若不存在 → 新增商品（默认库存1，上架时间为当前日期），返回"2"；

前端根据返回结果显示提示（如"商品上架成功"），并跳转至列表页。

---

五、注意事项与优化建议

代码错误：BrandServiceImpl.java中bandMapper拼写错误，应修正为brandMapper，否则会导致依赖注入失败。

分类数据静态化：goods-add.jsp的分类单选框为硬编码（如value="1"对应"家用电器"），建议改为动态从数据库加载，与品牌逻辑一致。

表单验证：前端添加页未对必填项（如商品名称、价格）做严格验证，建议补充JavaScript验证逻辑。

异常处理：Service和Controller层异常处理不完善，建议添加try-catch并返回友好提示。

SQL优化：GoodsMapper.xml中findGoodsAll使用select \*，建议明确指定字段，避免冗余数据查询。

---

六、关键配置摘要

分页逻辑：GoodsController中通过indexStart = (currentPage-1)\*pageSize计算分页起始位置，lastPage根据总记录数和页大小（5条/页）计算。

AJAX品牌加载：goods-add.jsp中通过$.ajax请求/brand/findBrands，成功后动态生成<option>标签。

字符编码：web.xml配置CharacterEncodingFilter，确保所有请求/响应使用UTF-8编码。

---

核心业务逻辑代码解析

1. 商品分页查询（GoodsController.java）

```plain
@RequestMapping("/list")
public String findGoodsAll(Model model,String currentPage){
    HashMap<Object, Object> hashMap = new HashMap<>();
    Integer pageSize =5; // 每页显示5条数据
    
    if (currentPage == null || currentPage.equals("")){
        currentPage = "1"; // 默认显示第一页
    }
    
    // 计算分页起始索引：(当前页-1)*每页条数
    Integer indexStart = (Integer.parseInt(currentPage)-1)*pageSize;
    hashMap.put("indexStart",indexStart);
    hashMap.put("pageSize",pageSize);
    
    // 查询商品列表（带分页参数）
    List<Goods> goodsList = goodsService.findGoodsAll(hashMap);
    model.addAttribute("goodsList",goodsList);
    return "goods-list";
}
```

代码说明：

分页核心公式：indexStart = (currentPage-1)\*pageSize，用于SQL的limit子句（如limit 0,5查询第1页，limit 5,5查询第2页）；

通过HashMap传递分页参数到Mapper层，最终在GoodsMapper.xml中拼接为limit #{indexStart},#{pageSize}。

2. 商品添加与库存更新逻辑（GoodsServiceImpl.java）

```plain
@Override
public String insertGoods(Goods goods) {
    if (goods.getGoodsName() !=null){
        // 检查商品是否已存在（按名称查询）
        Goods g = goodsMapper.findGoodsByName(goods.getGoodsName());
        
        if(g != null){ 
            // 商品已存在：库存+1
            int update = goodsMapper.updateGoodsNum(g.getGid());
            return update ==1 ? "1" : "3"; // 更新成功返回"1"，失败返回"3"
        }else{ 
            // 商品不存在：新增商品
            goods.setNum(1); // 默认库存1
            goods.setUpTime(new SimpleDateFormat("yyyy-MM-dd").format(new Date())); // 上架时间为当前日期
            int insert = goodsMapper.insertGoods(goods);
            return insert ==1 ? "2" : "3"; // 新增成功返回"2"，失败返回"3"
        }
    }
    return "3"; // 参数错误返回"3"
}
```

代码说明：

业务逻辑分支清晰：先查后增/改，避免重复添加商品；

使用三目运算符简化返回结果判断，前端根据返回值（"1"/"2"/"3"）显示不同提示（如"库存已增加"或"上架成功"）。

3. MyBatis关联查询（GoodsMapper.xml）

```plain
<resultMap id="goodsVo" type="com.eagles.ssm.pojo.Goods">
    <id column="gid" property="gid"/>
    <result column="goods_name" property="goodsName"/>
    <!-- 其他基本字段映射... -->
    
    <!-- 关联品牌表：通过brand_id关联ssm_brand表 -->
    <association property="brand" javaType="com.eagles.ssm.pojo.Brand">
        <id column="bid" property="bid"/> <!-- 品牌ID -->
        <result column="brand_name" property="brandName"/> <!-- 品牌名称 -->
    </association>
    
    <!-- 关联分类表：通过cate_id关联ssm_cate表 -->
    <association property="cate" javaType="com.eagles.ssm.pojo.Cate">
        <id column="cid" property="cid"/>
        <result column="cate_name" property="cateName"/>
    </association>
</resultMap>

<select id="findGoodsAll" parameterType="hashmap" resultMap="goodsVo">
    select * from ssm_goods sg
    left join ssm_brand sb on sg.brand_id = sb.bid <!-- 左连接品牌表 -->
    left join ssm_cate sc on sg.cate_id = sc.cid <!-- 左连接分类表 -->
    <if test="indexStart != null and pageSize != null">
        limit #{indexStart},#{pageSize} <!-- 分页条件 -->
    </if>
</select>
```

代码说明：

association标签实现多表关联查询，将ssm\_brand和ssm\_cate表的字段映射到Goods实体的brand和cate属性；

left join确保即使商品没有关联品牌/分类，也能正常查询出商品基本信息；

动态SQL（<if>标签）实现分页条件的按需拼接，避免SQL冗余。

四、关键问题与代码优化建议

1. 已识别的代码错误

BrandServiceImpl.java： private BrandMapper bandMapper; → 变量名拼写错误，应修正为brandMapper，否则Spring无法注入BrandMapper的代理对象，导致NullPointerException。

2. 代码优化示例

优化前：商品分页参数计算（硬编码）

```plain
Integer pageSize =5; // 硬编码，修改需改代码
```

优化后：配置化分页参数

```plain
// 在配置文件中定义（如db.properties）：page.size=5
@Value("${page.size}") 
private Integer pageSize; // 通过@Value注入，无需硬编码
```

|  |  |  |
| --- | --- | --- |
| **业务功能** | **核心代码位置** | **关键代码作用** |
| 品牌下拉框加载 | goods-add.jsp 的 AJAX 代码 + BrandController | 前后端数据交互，动态渲染选项 |
| 商品分页展示 | GoodsController 的 findGoodsAll 方法 | 计算分页参数，查询指定页数据 |
| 重复商品库存更新 | GoodsServiceImpl 的 insertGoods 方法 | 按名称查询商品，存在则更新库存 |
| 多表关联查询 | GoodsMapper.xml 的 resultMap（association） | 映射商品-品牌-分类的关联关系 |

##### 表格说明

1. **品牌下拉框加载**：涉及到前端 `goods-add.jsp` 页面中的 AJAX 调用和后端 `BrandController` 中相应的方法。这个过程主要用于实现页面加载时的品牌选择框动态填充。
2. **商品分页展示**：通过调用 `GoodsController` 中的 `findGoodsAll` 方法来处理商品分页逻辑。该方法负责计算分页参数，并返回给定页面的商品列表数据。
3. **重复商品库存更新**：在 `GoodsServiceImpl` 类中定义的 `insertGoods` 方法实现了检查商品是否已存在于数据库中，如果存在，则更新其库存数量的功能。
4. **多表关联查询**：使用 MyBatis 的 `resultMap` 特性，在 `GoodsMapper.xml` 文件中配置了商品、品牌和分类之间的复杂关联关系映射，以便执行高效的多表查询操作。
