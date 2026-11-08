---
title: "JavaScript"
yuque_slug: "rmz0guxfh7ua8var"
yuque_id: 229064570
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/rmz0guxfh7ua8var"
---
- **JavaScript语言诞生主要是完成页面的数据验证，因此它运行在客户端，需要运行浏览器来解析执行JavaScript代码 ，简称JS**
- **JS是Netscape网景公司的产品，最早取名为LiveScript 为了吸引更多java开发人员，更名为 JavaScript，JS是弱类型，Java是强类型 注释形式和java相同【“// "】**

1. 使用script标签来书写JavaScript代码
2. 使用script标签引入单独的JavaScript代码文件

```plain
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
1.<!--引入外部js文件-->
<script src ="./hello.js"></script>

2.<!--js脚本标签-->
<script>
    //网页弹窗
    alert('hello js!')
    console.log('hello world!') //网页的输出台

</script>

<body>

</body>
</html>
```

我们需要结合html来编写js代码：

a、在html文件中定义一个 <script></script> 标签，在该标签的内部去写js代码

```html
<!DOCTYPE html> 
<html lang="en"> 
  <head> 
    <meta charset="UTF-8"> 
    <title>JavaScript</title> 
  </head> 
  <!--定义一个script去写js代码--> 
  <script> 
    //js中封装的有对应的函数 
    alert("网页弹窗"); 
  </script> 
  <body> 
  </body> 
</html> 
```

b、会声明一个js文件，在js文件中去写js代码，然后通过 <script> 标签去引入js文件

```html
<!--引入js文件--> 
<script type="text/javascript" src="../js/test.js"></script>
```

**JavaScript****变量**

数值类型：number

字符串类型：string

对象类型：object

布尔类型：boolean

函数类型：function

定义变量

-let 全局变量

-var 局部变量

**函数的两种定义方式**

-let 函数名称 =function（形式参数列表）{  
 函数体内容

return data；

}

```html
function function01(num1,num2){ 
let num = num1 + num2; 
return num; 
}
```

-function 函数名称（形式参数列表）{  
 函数体内容

return data；

}

```html
let function02 = function(num1,num2){
let num = num1 + num2;
return num;
}
```

**JS****隐式参数**

不传参数可以通过**agruments**获取到传进函数的值

```html
//arguments js 中的隐式参数，是一个数组类型，可以去接收所有数据
function function02(){
alert(arguments[0]);
alert(arguments[1]);
alert(arguments[2]);
}
alert(function02(100,true,"hello"));
```

**简单的数字相加函数**

```html
let function02 = function(num1,num2) {
let num = null;
if ((typeof num1) == "number" && (typeof num2) == "number") {
  let num = num1 + num2;
} else {
 return "你传的数据不合法";
}
 return num;
}
```

**关于****js****中的事件（分为静态注册和动态注册）：**

**静态注册**：通过html标签内部的事件属性来完成事件的注册

```html
<body onload="alert('静态注册页面加载完成事件')"> </body>
```

```html
<!--鼠标单机事件：静态注册-->
<input type="button" value="测试鼠标单机事件" onclick="alert('鼠标单机事件静态注
册')">
```

**动态注册**：先通过dom对象获取到某一个具体的html元素，然后对这个标签进行绑定，注册事件

document.getElementById() 通过id这个属性查找html元素

**注**：动态注册一定要写在 script 标签中，或者js文件中

```html
//页面加载完成事件动态注册
//window是html内置对象
window.onload = function (){
//在该函数中完成页面初始化的操作
alert("动态绑定页面加载完成事件")
}
```

动态注册单击事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>JavaScript</title>
</head>
<!--定义一个script去写js代码-->
<script type="text/javascript">
//页面加载完成事件动态注册
//window是html内置对象
window.onload = function (){
alert("动态绑定页面加载完成事件")
//通过动态注册的方式来注册单击事件
let elementInput = document.getElementById("onclickTest");
//[object HTMLInputElement] dom对象
alert(elementInput)
//动态注册单击事件
elementInput.onclick=function (){
alert("触发了单击事件，动态注册的方式")
}
}
</script>
<!--引入js文件-->
<!--<script type="text/javascript" src="../js/test.js"></script>-->
<body>
<!--鼠标单机事件：静态注册-->
<input type="button" value="测试鼠标单机事件(静态)" onclick="alert('鼠标单机
事件静态注册')"><br>
<input type="button" value="测试鼠标单机事件(动态)" id="onclickTest"><br>
</body>
</html>
```

加载完成事件（**onload**）：

浏览器在解析html页面的时候是一个标签一个标签的往下解析

在浏览器解析完所有的html元素之后会触发的操作

失去焦点事件（**onblur**）

```html
测试鼠标失去焦点事件(静态):<input type="text" id="onblur" onblur="alert('触发了失去
焦点事件')"><br>
```

```html
//动态绑定失去焦点事件
let elementOnblur = document.getElementById("onblurTest");
//绑定事件
elementOnblur.onblur = function (){
alert("触发了失去焦点事件，动态注册的方式");
}
```

内容发送改变事件（**onchange**）

```html
测试内容发生改变事件(静态):<input type="text" value="123" onchange="alert('内容已发
生改变')"><br>
```

```html
//内容发生改变事件
let elementSelect = document.getElementById("onchangeTest");
//绑定事件
elementSelect.onchange = function (){
alert("内容已发生改变,动态注册的方式");
}
```

**JS****对象**

在js中也能new对象，但是必须使用js封装好的对象

通过 对象.属性 = 值 的方法赋值，属性和值都可以自定义

```html
let object = new Object();
object.name = "tom";
object.age = 15;
object.address = "北京市";
object.key = "value";
alert(object.name);
alert(object.age);
alert(object.address);
//正则对象
let regExp = new RegExp();
```

**JS****正则表达式用法**:

用双引号的正则表达式需要对特殊字符进行转义

用"\"符号的正则表达式则标识表达式不需要转义

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #0d00f8;
            display: flex;
            flex-direction: column; /* 垂直排列子元素 */
            align-items: center; /* 水平居中 */
            min-height: 100vh;
            margin: 0;
        }

        h1 {
            margin-top: 20px; /* 顶部留出一些间距 */
        }

        table {
            background-color: #fd0000;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 400px;
        }

        table td {
            padding: 10px;
        }

        label {
            font-weight: bold;
        }

        input[type="text"],
        input[type="password"],
        input[type="tel"],
        input[type="email"] {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        input[type="submit"] {
            width: 100%;
            padding: 10px;
            background-color: #007BFF;
            color: #ffffff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }

        input[type="submit"]:hover {
            background-color: #0056b3;
        }
    </style>
</head>

<!--引入外部的js文件-->
<script src="./reg.js"></script>

<body>
<h1>注册</h1>
<form action="#">
    <table>
        <tr>
            <td><label for="username">用户名:</label></td>
            <td>
                <input type="text" id="username" onblur="usernameReg()" name="username" required>
                <span id="usernameSpan"></span>
            </td>
        </tr>
        <tr>
            <td><label for="password">密码:</label></td>
            <td>
                <input type="password" id="password" onblur="password1Reg()" name="password" required>
                <span id="passwordSpan1"></span>
            </td>
        </tr>
        <tr>
            <td><label for="confirmPassword">确认密码:</label></td>
            <td>
                <input type="password" id="confirmPassword" onblur="password2Reg()" name="confirmPassword" required>
                <span id="passwordSpan2"></span>
            </td>
        </tr>
        <tr>
            <td><label for="phone">手机号:</label></td>
            <td>
                <input type="tel" id="phone" onblur="phoneReg()" name="phone" required>
                <span id="phoneSpan"></span>
            </td>
        </tr>
        <tr>
            <td><label for="email">邮箱:</label></td>
            <td>
                <input type="email" id="email" onblur="emailReg()" name="email" required>
                <span id="emailSpan"></span>
            </td>
        </tr>
        <tr>
            <td colspan="2">
                <input type="submit" value="注册">
            </td>
        </tr>
    </table>
</form>
</body>
</html>
```

```javascript
function usernameReg(){

  //创建正则对象
  let regExp = new RegExp("^[a-zA-Z0-9_]{4,20}$");

  let username = document.getElementById("username").value

  if (regExp.test(username)){
    document.getElementById("usernameSpan").innerText = "用户名可用";
    document.getElementById("usernameSpan").style = "color : green";
  }else if (username != ""){
    document.getElementById("usernameSpan").innerText = "用户不合法";
    document.getElementById("usernameSpan").style = "color : red";
  }else if(username == ""){
    document.getElementById("usernameSpan").innerText = "";
  }
}

//差不多的方法，有小部分不同
// function usernameReg() {
//     const usernameInput = document.getElementById('username');
//     const usernameSpan = document.getElementById('usernameSpan');
//     const username = usernameInput.value;
//     const regex = /^[a-zA-Z0-9]{4,16}$/; // 假设用户名由 4 - 16 位字母或数字组成
//
//     if (regex.test(username)) {
//         usernameSpan.textContent = '用户名格式正确';
//         usernameSpan.style.color = 'green';
//     } else {
//         usernameSpan.textContent = '用户名格式错误，需 4 - 16 位字母或数字';
//         usernameSpan.style.color = 'red';
//     }
// }

function password1Reg(){

  //创建正则对象
  let regExp = new RegExp("^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@$!%*?&])[A-Za-z\\d@$!%*?&]{8,16}$");

  let password = document.getElementById("password").value

  if (regExp.test(password)){
    document.getElementById("passwordSpan1").innerText = "密码可用";
    document.getElementById("passwordSpan1").style = "color : green";
  }else if (password != ""){
    document.getElementById("passwordSpan1").innerText = "密码不合法";
    document.getElementById("passwordSpan1").style = "color : red";
  }else if (password == ""){
    document.getElementById("passwordSpan1").innerText = "";
  }

}

function password2Reg(){

  let password1 = document.getElementById("password").value
  let password2 = document.getElementById("confirmPassword").value

  if (password1 == password2 && password2 != ""){
    document.getElementById("passwordSpan2").innerText = "密码一致";
    document.getElementById("passwordSpan2").style = "color : green";
  }else if (password2 != ""){
    document.getElementById("passwordSpan2").innerText = "密码不一致";
    document.getElementById("passwordSpan2").style = "color : red";
  }else if (password2 == ""){
    document.getElementById("passwordSpan2").innerText = "";
  }

}

function phoneReg(){

  //创建正则对象
  let regExp = new RegExp("^1[3-9]\\d{9}$");

  let phone = document.getElementById("phone").value

  if (regExp.test(phone)){

    document.getElementById("phoneSpan").innerText = "手机号可用";
    document.getElementById("phoneSpan").style = "color : green";
  }else if (phone != ""){
    document.getElementById("phoneSpan").innerText = "手机号不合法";
    document.getElementById("phoneSpan").style = "color : red";
  }else if (phone == ""){
    document.getElementById("phoneSpan").innerText = "";
  }

}

function emailReg(){

  //创建正则对象
  let regExp = new RegExp("^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$");

    let email = document.getElementById("email").value

    if (regExp.test(email)){

        document.getElementById("emailSpan").innerText = "邮箱可用";
        document.getElementById("emailSpan").style = "color : green";
    }else if (email != ""){
        document.getElementById("emailSpan").innerText = "邮箱不合法";
        document.getElementById("emailSpan").style = "color : red";
    }else if (email == ""){
        document.getElementById("emailSpan").innerText = "";
    }

}
```

**数组：**

var 数组名 = []; //空数组

var 数组名 = [1,'abc',true]; //定义数组同时赋值元素

js数组可以在一个数组中存放任意类型的值

js数组可以给任意一个下标赋值，未赋值的默认为**undefined**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>数组</title>
  </head>

  <script>
    //声明一个数组
    let array =[100,200,"hello",false,{},new Object()];

    array[9]=1000;
    alert(array.length) //10

    for ( let i =0;i<array.length;i++){
      alert("第"+(i+1)+"个元素为："+array[i])
    }

  </script>

  <body>

  </body>
</html>
```

案例：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <script>
    /*
  将 5 个图书对象存储到数组中，然后求得图书的价格总和
   */

    let bookArray=[
      {bookName:"三国演义",price:150,num:20},
      {bookName:"水浒传",price:120,num:20},
      {bookName:"西游记",price:180,num:28},
      {bookName:"红楼梦",price:190,num:30},
      {bookName:"死了",price:130,num:23}
    ];

    let sum =0;
    //对图书的数组进行遍历
    for (let i = 0; i <bookArray.length ; i++) {

      //价格的累加
      sum +=bookArray[i].price;

    }
    alert("图书总价格为："+sum)
  </script>

  <body>

  </body>
</html>
```

-js中不支持方法的重载【方法名称相同，参数列表不同】，下面函数会覆盖上面同名的函数

-js函数中参数，不需要指定参数类型

-js中的返回值，没有返回值可以不用写return关键字，return关键字可写可不写

-js中每个函数都有内置对象arguments，用来记录参数列表的每一个数值。js 中的隐式参数，是一个数组类型，可以去接收所有数据

-在js中 null、undefined、0 、false参与逻辑运算时本身代表的是false，其余所有的数据都表示true

-js中的函数，在定义参数的时候可以不用写数据类型，参数与参数之间用逗号分割
