---
title: "JSP"
yuque_slug: "cdrw0gzklgl96rg3"
yuque_id: 228670550
yuque_updated: "2026-10-14T04:54:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/cdrw0gzklgl96rg3"
---
JSP（JavaServer Pages）是一种用于创建动态Web页面的技术，它允许开发者在HTML、XML等文档中嵌入Java代码。以下是关于JSP的常用方法和书写格式的基本介绍：

### JSP 基本语法

1. **指令标签**：用来定义页面的一些属性或导入一些包。

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

2. **声明**：在JSP页面中声明变量和方法。

```html
<%! int count = 0; %>
  <%! public String sayHello(String name) { return "Hello, " + name; } %>
```

3. **表达式**：将Java表达式的值输出到客户端。

```html
<%= new java.util.Date() %>
```

4. **脚本元素**：在页面中嵌入Java代码片段。

```html
<%
  String userName = request.getParameter("userName");
  if (userName != null && !userName.isEmpty()) {
  out.println("Welcome, " + userName);
  }
  %>
```

5. **动作标签**：使用标准的动作标签来执行一些操作，如转发请求、包含文件等。

```html
<jsp:include page="header.jsp" />
  <jsp:forward page="errorPage.jsp" />
```

### JSP 内置对象

JSP提供了几个内置对象，可以直接在JSP页面中使用而无需声明：

- `request`: HttpServletRequest对象，用于获取HTTP请求信息。
- `response`: HttpServletResponse对象，用于设置响应内容。
- `session`: HttpSession对象，用于维护用户会话。
- `out`: PrintWriter对象，用于向客户端发送文本数据。
- `application`: ServletContext对象，用于共享应用程序级别的信息。
- `config`: ServletConfig对象，提供初始化参数。
- `page`: 指当前页面的对象，相当于Java中的`this`关键字。
- `pageContext`: PageContext对象，用于管理页面范围内的属性和控制流程。

### 示例

以下是一个简单的JSP页面示例，展示了如何使用上述概念：

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
  <head>
    <title>My First JSP Page</title>
  </head>
  <body>
    <h1>Welcome to My JSP Page</h1>
    <%
      String message = "Hello, World!";
      out.println("<p>" + message + "</p>");
      %>
      <p>Today's date: <%= new java.util.Date() %></p>
  </body>
</html>
```

这个例子展示了如何使用表达式和脚本来生成动态内容，并且也包含了基本的HTML结构。

### 演示jsp的工作原理 底层其实就是一个servlet

```java
package com.eagles.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
//演示jsp的工作原理 底层其实就是一个servlet
@WebServlet("/test")
public class TestServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");

        PrintWriter writer = response.getWriter();

        writer.print("<html>");
        writer.print("<head>");
        writer.print("<title>test</title>");
        writer.print("</head>");
        writer.print("<body>");
        writer.print(" success!");
        writer.print(" </body>");
        writer.print(" </html>");
    }
}
```

案例：

```plain
<%@ page import="com.eagles.pojo.Emp" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--jsp中的注释--%>
<%--
起到注释的效果
--%>

<%--声明变量--%>
<%!
 String name ="jack";
 int age =20;
%>

<%--代码脚本--%>
<%
   //String name ="Tom";

    //创建对象
    Object obj = new Object();
    Emp emp = new Emp();

    for (int i = 0; i <10 ; i++) {

    }
%>

<html>
<head>
    <title>text</title>
</head>
<body>

<!--html注释-->

<%--取值脚本--%>
<%=name%>
<%=age%>
<%=emp%>
<%=obj%>

</body>
</html>
```

```plain
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--代码脚本--%>
<%

    /*   pageContext:jsp内置的域对象，只能在当前的页面才能取到数据
       request:取值范围:一次请求范围内（只要地址不发生变化，都能取到request域中的值）
       session：会话对象，取值范围: 浏览器的开启到关闭，只要浏览器不关闭，都能取到session域中的值
       application：ServletContext，取值范围：整个Web工程
    */

    pageContext.setAttribute("pageContext","pageContext");
    request.setAttribute("request","request");
    session.setAttribute("session","session");
    application.setAttribute("application","application");
%>
<html>
<head>
    <title>test</title>
</head>
<body>
<%=request.getAttribute("request")%>
<%=pageContext.getAttribute("pageContext")%>
<%=session.getAttribute("session")%>
<%=application.getAttribute("application")%>
</body>
</html>
```

```plain
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>test</title>
</head>
<body>
<%=request.getAttribute("request")%>
<%=pageContext.getAttribute("pageContext")%>
<%=session.getAttribute("session")%>
<%=application.getAttribute("application")%>
</body>
</html>
```

```plain
<%@ page import="com.eagles.pojo.Emp" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--代码脚本--%>
<%
  //创建一个员工对象
    Emp emp = new Emp();
    emp.setEmpName("jack");
    emp.setAge(20);
    emp.setSex(1);

    request.setAttribute("emp",emp);

    //创建一个集合对象
    ArrayList<String> stringArrayList = new ArrayList<>();
    stringArrayList.add("123");
    stringArrayList.add("456");
    stringArrayList.add("789");

    session.setAttribute("stringArrayList",stringArrayList);

    //创建一个集合对象
    HashMap<Object, Object> objectObjectHashMap = new HashMap<>();
    objectObjectHashMap.put("key1","value1");
    objectObjectHashMap.put("key2","value2");
    objectObjectHashMap.put("key3","value3");
    objectObjectHashMap.put("key4","value4");

    application.setAttribute("objectObjectHashMap",objectObjectHashMap);

%>

<html>
<head>
    <title>test</title>
</head>
<body>
${message}<br>
${emp}<br>
${emp.empName}<br>
${emp.age}<br>
${stringArrayList}<br>
<!--取第一个下表索引的数据-->
${stringArrayList[0]}<br>
${stringArrayList[1]}<br>
<!--根据key获取value-->
${objectObjectHashMap.key1}<br>
${objectObjectHashMap.key2}<br>
${objectObjectHashMap.key3}<br>
${emp.sex == 1?'男': '女'}<br>
<%--是否存在--%>
${empty   emp}<br>
<%--是否不存在--%>
${not empty emp}<br>
</body>
</html>
```

```plain
<%@ page import="com.eagles.pojo.User" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="com.eagles.pojo.Emp" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--导入jstl标签库--%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"  %>

<%--代码脚本--%>
<%
//创建user对象
    User user = new User();
    user.setUsername("张三");

    ArrayList<Emp> empArrayList = new ArrayList<>();
    Emp emp1 = new Emp();
    emp1.setSex(1);
    emp1.setAge(20);
    emp1.setBirthday("1998");
    emp1.setAddress("镇江市");
    emp1.setEmpName("jack");

    Emp emp2 = new Emp();
    emp2.setSex(1);
    emp2.setAge(20);
    emp2.setBirthday("1998");
    emp2.setAddress("镇江市");
    emp2.setEmpName("jack");

    Emp emp3 = new Emp();
    emp3.setSex(1);
    emp3.setAge(20);
    emp3.setBirthday("1998");
    emp3.setAddress("镇江市");
    emp3.setEmpName("jack");

    empArrayList.add(emp1);
    empArrayList.add(emp2);
    empArrayList.add(emp3);

    request.setAttribute("empArrayList",empArrayList);
    //session.setAttribute("user",user);
%>

<html>
<head>
    <title>test</title>
</head>
<body>
<%--var：容器中每个对象的别名  items：遍历的容器--%>
<c:if test="${not empty user}">
<c:forEach var="emp" items="${empArrayList}">

    ${emp.sex}&nbsp;&nbsp;&nbsp;
    ${emp.age}&nbsp;&nbsp;&nbsp;
    ${emp.empName}&nbsp;&nbsp;&nbsp;
    ${emp.address}&nbsp;&nbsp;&nbsp;
    ${emp.birthday}&nbsp;&nbsp;&nbsp;

</c:forEach>
</c:if>

<c:if test="${empty user}">
    <a href="您当前还没有登录，请前往登录页面进行登录"></a>
</c:if>
</body>
</html>
```
