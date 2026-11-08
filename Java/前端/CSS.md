---
title: "CSS"
yuque_slug: "tzwdkm2gsapu63xr"
yuque_id: 229011898
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/tzwdkm2gsapu63xr"
---
### CSS简介

CSS（层叠样式表，Cascading Style Sheets）是一种用于描述HTML或XML文档外观和格式的样式表语言。通过CSS，开发者可以控制网页的内容如何展示，包括布局、颜色、字体等视觉方面。它使得网页设计更加灵活，允许内容与表现分离，从而提高代码的可维护性和复用性。

### 常见CSS方法

#### 1. **选择器（Selectors）**

- **元素选择器**：根据标签名选择元素。

```css
p { color: blue; }
```

- **类选择器**：使用`.classname`选择具有特定类的所有元素。

```css
.highlight { background-color: yellow; }
```

- **ID选择器**：使用`#idname`选择具有特定ID的元素。

```css
#header { font-size: 20px; }
```

- **后代选择器**：选择作为另一元素后代的元素。

```css
div p { color: green; }
```

#### 2. **属性（Properties）**

- **颜色属性**：如`color`, `background-color`
- **尺寸属性**：如`width`, `height`, `padding`, `margin`
- **位置属性**：如`position`, `top`, `left`, `right`, `bottom`
- **显示属性**：如`display`, `visibility`

#### 3. **值（Values）**

- **颜色值**：如`red`, `#ff0000`, `rgb(255,0,0)`
- **长度值**：如`px`, `em`, `rem`, `%`
- **关键字**：如`auto`, `inherit`

### 案例

#### 案例1：基础样式设置

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>CSS Example</title>
    <style>
      body { font-family: Arial, sans-serif; }
      h1 { color: navy; margin-left: 20px; }
      p { font-size: 14px; color: gray; }
    </style>
  </head>
  <body>
    <h1>Welcome to My Webpage</h1>
    <p>This is a paragraph of text.</p>
  </body>
</html>
```

#### 案例2：利用类和ID进行更复杂的样式设计

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>CSS Example with Classes and IDs</title>
    <style>
      .container { width: 80%; margin: auto; padding: 20px; border: 1px solid black; }
      .highlight { background-color: yellow; }
      #special { font-weight: bold; color: red; }
    </style>
  </head>
  <body>
    <div class="container">
      <p class="highlight">This is a highlighted paragraph.</p>
      <p id="special">This is a special paragraph.</p>
    </div>
  </body>
</html>
```

#### 案例3：响应式设计

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Responsive Design Example</title>
    <style>
      @media (max-width: 600px) {
        body { background-color: lightblue; }
      }
      @media (min-width: 601px) {
        body { background-color: lightgreen; }
      }
    </style>
  </head>
  <body>
    <p>Resize the browser window to see the effect.</p>
  </body>
</html>
```

#### 案例4：css代码的引入方式

```css
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>css</title>
<!--引入外部的css文件-->
<link rel="stylesheet" href="../css/style.css"/>

<!--行内样式表-->
<style>
#box1{
  background-color: #1af13a;
  width: 100px;
  height: 100px;
}

/*通过css选择器来选择页面的元素*/
.box{
  background-color: #1b3cc4;
  width: 100px;
  height: 100px;
}

/*标签选择器*/
body{
  background-color: #12d5b2;
}

</style>

</head>
<body>
<div style="background-color: red;width: 100px;height: 100px">
1234
</div>

<div id="box1">
1234
</div>

<div class="box">
1234
</div>

<div class="box">
1234
</div>

<div class="box">
1234
</div>

<div class="box">
1234
</div>

<div id="box2">
通过外部样式表来引入css样式
</div>

</body>
</html>
```

```css
#box2{
  width: 200px;
  height: 200px;
  background-color: blue;
  border: 10px solid black;
}
```

#### 案例5：搜索框以及按钮的美化

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>案例分析</title>

    <style>
      input[type="button"] {
        background-color: #007BFF; /* 设置按钮背景颜色 */
        color: white; /* 设置按钮文字颜色 */
        padding: 8px 16px; /* 设置按钮内边距 */
        border: none; /* 移除按钮边框 */
        border-radius: 4px; /* 设置按钮圆角 */
        cursor: pointer; /* 鼠标悬停时显示手型光标 */
        transition: background-color 0.3s ease; /* 添加背景颜色过渡效果 */
      }

      input[type="button"]:hover {
        background-color: #0056b3; /* 鼠标悬停时按钮背景颜色 */
      }

      input[type="text"] {
        padding: 8px; /* 设置输入框内边距 */
        border: 1px solid #ccc; /* 设置输入框边框 */

        /*内边距*/
        border-radius: 4px; /* 设置输入框圆角 */

        /*外边距*/
        margin-right: 8px; /* 设置输入框与按钮之间的间距 */
      }
    </style>

  </head>
  <body>

    <h1>案例分析</h1>
    <input type="text">
    <input type="button" value="搜索">

  </body>
</html>
```
