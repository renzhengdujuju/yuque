---
title: "FileWriter"
yuque_slug: "nh9r5w2ve8h5gabo"
yuque_id: 220065525
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/nh9r5w2ve8h5gabo"
---
系统C盘里面之前并没有data01.txt文件，如果原电脑没有该文件，程序运行时将会被创建出来。FileWriter里面包含好几种写的Weiter方法

![](../_assets/7b4ac0c5_1747552542238-9f16e44c-fc75-473f-ab6c-88d98857d1ba.png)

```java
package com.io;

import java.io.FileWriter;
import java.io.IOException;

public class FileWriterTest {

    public static void main(String[] args) {

        FileWriter fileWriter = null;

        try {
            //创建字符输出流
            fileWriter = new FileWriter("C:\\temp\\data01.txt",false);

            //写ascll
            fileWriter.write(65);

            fileWriter.write("\n");
            //创建字符数组并写入对应的数据
            char[] chars = {'h','e','l','l','o'};

            char[] chars02 = {'w','o','r','l','d','1','2','3'};

            fileWriter.write(chars);

            fileWriter.write(" ");

            //按照字符数组的区间写入
            fileWriter.write(chars02,0,5);

            fileWriter.write("\n");

            fileWriter.write("你好");

            fileWriter.write("\n");

            fileWriter.write("你好世界",2,2);

            //进行刷新
            fileWriter.flush();

        } catch (IOException e) {
            throw new RuntimeException(e);
        }finally {
            if (fileWriter != null) {
                try {
                    fileWriter.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

![](../_assets/24ee2033_1747553280259-e45aa811-08c3-461b-8b32-6f87e84ce1d4.png)

成功输入进入数据

![](../_assets/2e597a4b_1747553336746-e9d35558-f2c4-4005-8afd-a70f626ec878.png)
