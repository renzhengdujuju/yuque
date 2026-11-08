---
title: "BufferedReader"
yuque_slug: "eivcsmul39vpwwhu"
yuque_id: 220100422
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/eivcsmul39vpwwhu"
---
独特方法readLine，其他大同小异

```java
package com.io;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;

/**
 * 缓冲流
 *      -包装流
 *      -节点流
 */
public class BufferedReaderTest {

    public static void main(String[] args) throws Exception {

        //创建缓冲流对象 (包装流和节点流都是相对而言的例如InputStreamReader相对于FileInputStream是包装流，而相对于BufferedReader是节点流)
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(new FileInputStream("C:\\temp\\CopyTest02.java")));

        //一次读取一行数据
        String readLine = null;

        while ((readLine = bufferedReader.readLine()) != null){

            System.out.println(readLine);

        }

        //关闭资源
        bufferedReader.close();

    }

}
```

创建成功

![](../_assets/9c3488de_1747555760931-26976019-3deb-4a3c-8615-55f101c51fa1.png)
