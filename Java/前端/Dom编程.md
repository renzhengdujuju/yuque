---
title: "Dom编程"
yuque_slug: "ozxubkszxc8kkq2w"
yuque_id: 229596511
yuque_updated: "2026-10-14T04:54:24.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/ozxubkszxc8kkq2w"
---
```plain
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>dom编程</title>
</head>
<script>
    //文本就绪函数 在网页加载完成后执行代码
    window.onload = function (){
        let objEl =document.getElementById("text01");
        alert(objEl)

        //根据标签名称获取标签对象
        let inputList = document.getElementsByName("input");
       //遍历的方法一
        for ( let i in inputList){
            alert(inputList[i])
        }
        //遍历的方法二
        for (let i = 0; i < inputList.length; i++) {

            alert(inputList[i])
        }

        let hobbyList = document.getElementsByName("hobby");
        alert(hobbyList.length)

       //获取该元素的所有子元素节点集合
        let childrenDom = document.getElementById("hobby").children;
        for (let i = 0; i < childrenDom; i++) {
            alert(childrenDom[i])
        }

    }

</script>

<body id="boddy">
    <input type="text" id="text01">
    <input type="text" id="text02">
    <input type="text" id="text03"><br>

    1<input type="checkbox" class="hobby" name="hobby">
    2<input type="checkbox" class="hobby" name="hobby">
    3<input type="checkbox" class="hobby" name="hobby">
    4<input type="checkbox" class="hobby" name="hobby">
</body>
</html>
```
