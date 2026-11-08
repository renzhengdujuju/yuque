---
title: "Jquery"
yuque_slug: "omi6mnflrrful6gm"
yuque_id: 229234407
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/omi6mnflrrful6gm"
---
-jquery底层封装了js，简化js的开发，是一个js库，核心函数是$

注意：jquery对象和dom对象的属性和方法不能相互调用

### query的常用方法

全选、全不选、反选

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>test</title>
  </head>
  <script src="../jquery/jquery-1.7.2.js"></script>
  <script>
    $(function (){
      //全选
      $("#qx").click(function (){
        $("[type ='checkbox']").each(function (){
          $(this).prop("checked",true)
        })
      })
      //全不选
      $("#qbx").click(function (){
        $("[type ='checkbox']").each(function (){
          $(this).prop("checked",false)
        })
      })
      //反选
      $("#fx").click(function (){
        $("[type ='checkbox']").each(function (){
          $(this).prop("checked",!this.checked)
        })
      })
    }) 
  </script>
  <body>

    1、<input type="checkbox" class="hobby">
    2、<input type="checkbox" class="hobby">
    3、<input type="checkbox" class="hobby">
    4、<input type="checkbox" class="hobby">
    5、<input type="checkbox" class="hobby"><br/>

    <input type="button" id="qx" value="全选">
    <input type="button" id="qbx" value="全不选">
    <input type="button" id="fx" value="反选">

  </body>
</html>
```

一键全选、全不选

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>test</title>
</head>
 <script src="../jquery/jquery-1.7.2.js"></script>
  <script>
    $(function (){
//全选、选不选按钮 点击全选，不点就全不选
    $("#cks").click(function (){
       let flag = this.checked;
    $(".hobby").each(function (){
    $(this).prop("checked",flag)
  })
})
</script>
<body>   id名随便取
  全选/全不选<input id="cks" type="checkbox"><br> 
```

下拉框

```html
<select id="cls">
  <option>--请选择你的英雄--</option>
  <option>--韩信--</option>
</select>
```

<div> 是一个通用的容器标签，本身不具备特定的语义，主要用于组织和包裹其他 HTML 元素，方便通过 CSS 或 JavaScript 进行样式设置和交互操作

```html
<div style="width: 50px;height: 50px;background-color: #007BFF" id="box">

</div>
```

属性过滤选择器 $("[ 属性='名字']")

```html
$("[class='hobby']").each(function (){
 alert(this)
})
```

类选择器 $(".类名")

```plain
//class选择器
let hobbyObj = $(".hobby");
```

### dom和jquery文本就绪函数的区别

```html
//dom的文本就绪函数
window.onload=function(){
document.getElementById("bt01").onclick = function (){
alert("单击绑定事件")
}
}
//jquery的文本就绪函数
$(function (){
alert($("#bt01")) //jquery对象
alert(document.getElementById("bt01")) //dom对象
```

$( ) 是 jQuery 函数的简写形式，将dom对象加入$( )将会被转化成jquery对象

```plain
//遍历多选框
hobbyObj.each(function (){
    this //当前遍历到的dom对象
    alert($(this)) //将对象转化成jquery对象
})
```

声明一个数组

```plain
let array =[100,200,"hello",false,{},new Object()];

array[9]=1000;
alert(array.length) //10 少的长度会自动补齐，显示为undefined

for ( let i =0;i<array.length;i++){
    alert("第"+(i+1)+"个元素为："+array[i])
}
```

在大多数场景下，{} 代表一个空对象。对象是 JavaScript 里一种非常重要的数据类型，能用来存储键值对。使用对象字面量语法可以简洁地创建对象。

在你当前编辑的代码里，let array =[100,200,"hello",false,{},new Object()]; 中的 {} 就是一个空对象，它和 new Object() 作用相同，不过 {} 语法更简洁

```plain
// 创建一个空对象
let emptyObj = {};

// 向对象中添加属性
emptyObj.name = "John";
emptyObj.age = 30;

// 访问对象属性
console.log(emptyObj.name); // 输出: John
console.log(emptyObj.age); // 输出: 30
```

数组的案例：

```plain
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
```

函数的声明

```plain
//-let 函数名称 =function（形式参数列表）
 let getFun01 = function (){
     alert("this is getFun01")
 }
 //-function 函数名称（形式参数列表）
 function getFun02(){
alert ("this is getFun02")
 }
 //-js中不支持方法的重载【方法名称相同，参数列表不同】，下面函数会覆盖上面同名的函数
 function getFun02(){
     alert ("1111~~~~")
 }
 function getFun03(name,age){
     alert("this is getFun03")
     alert(name)
     alert(age)
 }
 //参数可传可不传
 function  getFun04(name,age){
     alert("this is getFun04")
     return "success"
 }
 function getFun05(){
     alert(arguments[0])
     alert(arguments[1])
 }

 //函数的调用
         // getFun01()
        // getFun02()
       //getFun03()
      //var data = getFun04() //变量声明后未赋值，默认值就是 undefined。
     //alert(data)
   // getFun05("Tom",20)
```

使用 var 声明的变量可以在后续代码中被赋值、修改，还能多次赋值不同类型的数据。它具有函数作用域和变量提升的特性。

函数作用域

var 声明的变量在函数内部声明时，其作用域仅限于该函数内；若在函数外部声明，则为全局变量，在整个脚本中都能访问。示例如下：

```html
function exampleFunction() {
     var localVar = '这是局部变量';
     console.log(localVar); // 输出: 这是局部变量
}
exampleFunction();
// console.log(localVar); // 报错，localVar 在此作用域未定义

var globalVar = '这是全局变量';
  console.log(globalVar); // 输出: 这是全局变量
```

变量提升

使用 var 声明的变量会发生变量提升，即变量的声明会被提升到其所在作用域的顶部，但赋值不会提升。示例如下：

```html
console.log(tempVar); // 输出: undefined
var tempVar = '已赋值';

上述代码等同于：

var tempVar;
console.log(tempVar); // 输出: undefined
tempVar = '已赋值';
```

### 在网页的控制台打印

```html
console.log('hello world!')
```

### 事件

#### 单击事件

绑定单击事件 （jQuery）

```html
$("#bt01").click(function (){
    alert("通过jquery的方式绑定单击事件！")
})
```

静态绑定鼠标单击事件 onclick=" " （Dom）

```html
<input type="button" onclick="alert('静态绑定鼠标单击事件')"  value="测试鼠标单击事件">
```

动态绑定鼠标单击事件 （Dom）

```html
<script>

        //获取标签对象
        document.getElementById("bt01")
        // 绑定事件
       bt01.onclick =function (){
            alert("动态绑定鼠标单击事件")
       }
</script>   
<body>
 <input type="button" id="bt01" value="测试鼠标单击事件"><br>
  </body>
```

#### 页面加载完成的监听

```html
    window.onload=function (){
        alert("动态绑定页面加载完成事件!")
```

#### 内容改变事件

内容改变事件（动态）

```plain
//获取下拉框对象
        let selObj = document.getElementById("sel");
        selObj.onchange = function (){
            alert('动态绑定内容改变事件~~')
        }
<select id="sel"> 
                  <option>--请选择你的英雄--</option>
                  <option>--李白--</option>
                  <option>--张飞--</option>
                  <option>--韩信--</option>
               </select><br>
```

内容改变事件（静态）

```plain
onchange="test()"
```

#### 失去焦点事件

失去焦点事件（动态）

```plain
<script>
document.getElementById("text01").onblur = function (){

    alert('失去焦点事件触发了~~')
}
</script>
<body>
<input type="text" id="text01"><br/>
```

失去焦点事件（静态）

```plain
<   onblur="alert('失去焦点事件触发了~~')"   >
```

### typeof 效验变量是什么数据类型

```html
//typeof 效验变量是什么数据类型

//声明一个变量
let data ="hello";
alert(typeof data)//String

//变量可以再次被赋值
data = true;
alert(typeof data)//boolean

data = 100
alert(typeof data)//number

data =3.14
alert(typeof data)//number

let data2
alert(typeof data2)//undefined

let obj = new Object();
alert(typeof obj) //object

let obj2 = {name:"Tom",age:20,address:"南京市"}
alert(obj2.name)
alert(obj2.age)
alert(obj2.address)

data = function (){

}
alert(typeof  data)//function
```

### 简易版的计算器

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>计算器</title>
  </head>
  <script>
    function js(){
      let num1 = document.getElementById("num1").value
      let num2 = document.getElementById("num2").value

      //获取运算符
      let ysf = document.getElementById("ysf").value

      let num =0;

      if (ysf == "+"){
        num =Number(num1)+Number(num2)
      }else if (ysf == "-"){
        num =Number(num1)-Number(num2)
      }else if (ysf == "*"){
        num =Number(num1)*Number(num2)
      }else if (ysf == "/"){
        if (num2==0){
          alert("被除数不能为0")
        }
        num =Number(num1)/Number(num2)
      }
      //将结果回显
      document.getElementById("num").value = num;

    }

  </script>
  <body>
    <input type="text" id="num1" >
    <select id="ysf">
      <option value="+">+</option>
      <option value="-">-</option>
      <option value="*">*</option>
      <option value="/">/</option>
    </select>
    <input type="text" id="num2" >
    <input type="button" value="计算" onclick="js()">
    <input type="text" id="num" >

  </body>
</html>
```

### 测试：

```html
<!DOCTYPE html><!--对html5的声明-->
<html lang="en"><!--语言是english-->
  <head>
    <meta charset="UTF-8">
    <title>html的常用标签</title>
  </head>
  <body>

    <!--标题标签-->
    <h1>hello</h1>
    <h2>hello</h2>
    <h3>hello</h3>
    <h4>hello</h4>
    <h5>hello</h5>
    <h6>hello</h6>

    <!--下拉列表-->
    <select>
      <option>--请选择你的班级--</option>
      <option>--不想上班1班--</option>
      <option>--懒得上班2班--</option>
      <option>--不用上班3班--</option>
    </select>

    <!--段落标签-->
    <p>
      锄禾日当午
      汗滴禾下土
    </p>

    <p>
      锄禾日当午
      汗滴禾下土
    </p>

    <p>
      锄禾日当午
      汗滴禾下土
    </p>

    <!--图片嵌套标签-->
    <img style="width: 200px;height: 100px" src="../jpg/1%20-.jpg">
    <img style="width: 180px;height: 90px" src="../jpg/1%20-.jpg">
    <img style="width: 160px;height: 80px" src="../jpg/1%20-.jpg">
    <img style="width: 140px;height: 70px" src="../jpg/1%20-.jpg">
    <img style="width: 120px;height: 60px" src="../jpg/1%20-.jpg">
    <img style="width: 100px;height: 50px" src="../jpg/1%20-.jpg">
    <img style="width: 80px;height: 40px" src="../jpg/1%20-.jpg">
    <img style="width: 60px;height: 30px" src="../jpg/1%20-.jpg">
    <img style="width: 40px;height: 20px" src="../jpg/1%20-.jpg">
    <img style="width: 20px;height: 10px" src="../jpg/1%20-.jpg">

    <br>
    <!--图片嵌套标签-->
    <img style="width: 200px;height: 100px" src="../jpg/1%20-.jpg">
    <img style="width: 180px;height: 90px" src="../jpg/1%20-.jpg">
    <img style="width: 160px;height: 80px" src="../jpg/1%20-.jpg">
    <img style="width: 140px;height: 70px" src="../jpg/1%20-.jpg">
    <img style="width: 120px;height: 60px" src="../jpg/1%20-.jpg">
    <img style="width: 100px;height: 50px" src="../jpg/1%20-.jpg">
    <img style="width: 80px;height: 40px" src="../jpg/1%20-.jpg">
    <img style="width: 60px;height: 30px" src="../jpg/1%20-.jpg">
    <img style="width: 40px;height: 20px" src="../jpg/1%20-.jpg">
    <img style="width: 20px;height: 10px" src="../jpg/1%20-.jpg">
    <br>
    <!--图片嵌套标签-->
    <img style="width: 200px;height: 100px" src="../jpg/1%20-.jpg">
    <img style="width: 180px;height: 90px" src="../jpg/1%20-.jpg">
    <img style="width: 160px;height: 80px" src="../jpg/1%20-.jpg">
    <img style="width: 140px;height: 70px" src="../jpg/1%20-.jpg">
    <img style="width: 120px;height: 60px" src="../jpg/1%20-.jpg">
    <img style="width: 100px;height: 50px" src="../jpg/1%20-.jpg">
    <img style="width: 80px;height: 40px" src="../jpg/1%20-.jpg">
    <img style="width: 60px;height: 30px" src="../jpg/1%20-.jpg">
    <img style="width: 40px;height: 20px" src="../jpg/1%20-.jpg">
    <img style="width: 20px;height: 10px" src="../jpg/1%20-.jpg">

    <!--文本域-->
    <textarea>

    </textarea><br>

    <!--按钮-->
    <input type="submit">
    <input type="button" value="登录">

    <!--超链接标签-->
    <a href="https://www.baidu.com/">百度一下</a>

    <!--div标签（盒子）-->
    <div style="width: 100px;height: 100px;background-color: red">
      123
    </div>

    <div style="width: 100px;height: 100px;background-color: yellow">
      123
    </div>

    <!--span标签-->
    你好<span style="color: red">世界</span> <br/>

    <!--input 输入框-->
    <input type="text">
    <!--密码输入框-->
    <input type="password"><br/>
    <!--单选框-->
    男：<input type="radio" name="sex">
    女：<input type="radio" name="sex"><br/>
    <!--多选框-->
    敲代码：<input type="checkbox" name="hobby">
    打篮球：<input type="checkbox" name="hobby">
    睡觉：<input type="checkbox" name="hobby">
    约会：<input type="checkbox" name="hobby"><br/>

</body>
</html>
```
