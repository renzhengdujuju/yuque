---
title: "HTML"
yuque_slug: "naabwd4ht82f49se"
yuque_id: 229011911
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/naabwd4ht82f49se"
---
HTML 全称为 HyperText Markup Language，即超文本标记语言，它是构建网页的基础，用于创建网页的结构和内容。下面从多个方面详细介绍 HTML：

定义

HTML 不是编程语言，而是一种标记语言。它使用一系列标签（tag）来描述网页的不同部分，比如标题、段落、图片、链接等。浏览器会读取 HTML 文档，然后根据标签的含义将网页内容渲染显示出来。

### 基本结构

一个简单的 HTML 文档通常包含以下基本结构：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
<body>
    <!-- 页面内容 -->
    <h1>这是一个标题</h1>
    <p>这是一个段落。</p>
</body>
</html>
```

###### <!DOCTYPE html>：声明文档类型为 HTML5。

###### 1. <html>：根标签，包裹整个 HTML 文档。

###### 2. <head>：头部标签，包含一些关于页面的元信息，如字符编码、页面标题等，这些信息通常不会直接显示在页面上。

###### 3. <body>：主体标签，包含所有要在页面上显示的内容，如文本、图片、链接等。

### 常用标签

标题标签：<h1> 到 <h6> 用于定义不同级别的标题，<h1> 级别最高，字体最大。

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
```

段落标签：<p> 用于定义段落。

```html
<p>这是一个段落的内容。</p>
```

链接标签：<a> 用于创建超链接。

```html
<a href="https://www.example.com">这是一个链接</a>
```

图片标签：<img> 用于在页面上插入图片。

```html
<img src="example.jpg" alt="示例图片">
```

列表标签：包括有序列表 <ol> 和无序列表 <ul>，列表项使用 <li> 标签。

```html
<ul>
    <li>无序列表项1</li>
    <li>无序列表项2</li>
</ul>
<ol>
    <li>有序列表项1</li>
    <li>有序列表项2</li>
</ol>
```

### 与其他技术的关系

CSS：层叠样式表（Cascading Style Sheets），用于控制 HTML 文档的外观和布局，如颜色、字体、间距等。在你当前编辑的代码里，就使用了 <style> 标签内的 CSS 代码来美化按钮和输入框。

JavaScript：一种脚本语言，用于为网页添加交互功能，如表单验证、动态内容更新等。

### 文档笔记

|  |  |
| --- | --- |
| **标签类型** | **标签示例** |
| 标题标签 | `<h1></h1>` ~ `<h6></h6>` |
| 段落标签 | `<p></p>` |
| 图片嵌套标签 | `<img style="width: ?px; height: ?px;" src="?">`（`../` 表示向上一级目录）（px表示像素；src放照片的位置） |
| 文本域 | `<textarea></textarea>` |
| 下拉列表 | `<select></select>` |
| 选择项标签 | `<option></option>` |
| 按钮 | `<input type="submit">`  `<input type="button" value="登录">`  `<button></button>`  （默认 type 为 submit） |
| 超链接标签 | `<a href="https://www.baidu.com">百度一下</a>` |
| div 标签 | `<div style="background-color: red; width: 100px; height: 100px">1234</div>` |
| span 标签 | 你好<span style="background-color: red;">张三</span> |
| 输入框 | `<input type="text">` |
| 密码输入框 | `<input type="password">` |
| 多选框 | 敲代码：<input type="checkbox" name="hobby">  打篮球：<input type="checkbox" name="hobby">  睡觉：<input type="checkbox" name="hobby"> |
| 换行标签 | `<br />` |

**说明**：

- `../` 表示以当前文件夹为基础，向上一级目录查找资源。
- `<button>` 标签如果不写 `type`，默认行为是 `submit`
