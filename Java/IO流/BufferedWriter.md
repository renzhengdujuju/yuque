---
title: "BufferedWriter"
yuque_slug: "cb2q34wudqtsbems"
yuque_id: 220095364
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/cb2q34wudqtsbems"
---
```java
package com.io;

import java.io.BufferedWriter;
import java.io.FileWriter;

public class BufferedWriterTest {
    //先是抛出异常
    public static void main(String[] args) throws Exception {
        //最后这个文件路径不像之前那样是C盘里面的绝对路径，这次是开发在本地的相对路径
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter("data.txt"));

        bufferedWriter.write("hello world 你好 世界！");

        //强制将缓冲区的内容写到文件中，不然的话需要等到缓冲区满了，才会写到文件中
        bufferedWriter.flush();

        bufferedWriter.close();

    }
}
```

![](../_assets/086fccca_1747555611608-413f3015-2b5c-44a8-943f-cfbe9507c291.png)

成功创建

![](../_assets/e37946f1_1747555625693-38deadf4-d4e1-4e42-87cd-51b41de6f109.png)
