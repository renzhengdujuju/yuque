---
title: "Document"
yuque_slug: "sdk28e27bp2g4nlg"
yuque_id: 229140388
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/sdk28e27bp2g4nlg"
---
**关于html的上下文文本对象document**

**这个是html内置的对象，通过这个对象可以去获取html对象（标签）**

**而且获取的方式有很多种（比如：根据id属性获取，根据name属性获取，根据具体的标签名称获取）**

**DOM****模型和****document****对象**

**DOM**全称是Document Object Model 文档对象模型

**document**对象

1.Document它管理了所有的HTML文档内容

2.document它是一种树结构的文档，有层级关系

3.它让我们把所有的标签都对象化

4.我们可以通过document访问所有的标签对象

**Document****对象中的方法**

document.getElementById(elementId) 通过标签的id属性查找标签dom对象，elementId是标

签的id属性值

document.getElementsByName(elementName) 通过标签的name属性查找标签dom对象，

elementName标签的name属性值

document.getElementsByTagName(tagname) 通过标签名找标签dom对象，tagname是标签名

document.createElement(tagName) 方法，通过给定的标签名，创建一个标签对象，tagName是要创建的标签名

```html
//获取所有name属性值为username的标签
let inputEls = document.getElementsByName("username");
//[object NodeList]
alert(inputEls);
for (let i = 0; i < inputEls.length; i++) {
alert(inputEls[i]);
}
```

**在body中添加元素**

```html
elementById1.onclick = function (){
let htmlButtonElement = document.createElement("button");
let elementById2 = document.getElementById("body");
elementById2.appendChild(htmlButtonElement);
}
```

**标签的常用属性**

childNodes属性，获取当前节点的所有子节点

firstChild属性，获取当前节点的第一个子节点

lastChild属性，获取当前节点的最后一个子节点

parentNode属性，获取当前节点的父节点

nextSibling，获取当前节点的下一个节点

previousSibling属性，获取当前节点的上一个节点

className用于获取或设置标签的class属性值

innerHTML属性，表示获取/设置起始标签和结束标签中的内容

innerText属性，表示获取/设置起始标签和结束标签中的文本

```html
window.onload = function ()
//返回一个dom对象 
let elementBody = document.getElementById("body"); 
//获取所有的子节点 
let childNodes = elementBody.childNodes; 
for (let i = 0; i < childNodes.length; i++) { 
alert(childNodes[i]); 
} 
let firstChild = elementBody.firstChild; 
//文本对象 
alert(firstChild); 
let lastChild = elementBody.lastChild; 
alert(lastChild); 
let elementButton = document.getElementById("test1"); 
let parentNode = elementButton.parentNode; 
}
```
