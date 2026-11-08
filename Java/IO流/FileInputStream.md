---
title: "FileInputStream"
yuque_slug: "pns52am21vfoeq9e"
yuque_id: 220073590
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/pns52am21vfoeq9e"
---
![](../_assets/cfc37466_1747536708430-d839bb9b-80ee-4cdb-8721-fb5f998108c7.png)

首先在C盘创建一个文件和文本文档，并通过input对文件内容进行输入

![](../_assets/105b2c34_1747537090904-ea5c236a-14ea-4196-ad2e-ee63290a26b3.png)

在里面输入ABCDEFG

对文件里面的字符一个一个的进行打印，打印的对应的ascll

```java
package com.io;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileInputStreamTest {

    public static void main(String[] args) {

        FileInputStream fileInputStream = null;

        try {
            //创建字节输入流(后面跟对应所在的文件位置)
            fileInputStream = new FileInputStream("C:\\temp\\abc.txt");

            //读取文件中的数据（一个一个的读）
            int read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

            read = fileInputStream.read();
            System.out.println(read);

         //抛出存在的问题  + finally回收数据
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fileInputStream != null){
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

![](../_assets/42db7eef_1747536227002-f06c8a8b-75c3-4f76-a71d-bda929c6de82.png)

遍历数组，第一次没有遍历完全，遍历第二次，且第二次的字母会将第一次的字母覆盖

```java
package com.io;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileInputStreamTest2 {

    public static void main(String[] args) {

        FileInputStream fileInputStream = null;

        try {
            //创建字节输入流
            fileInputStream = new FileInputStream("C:\\temp\\abc.txt");

            //创建byte数组
            byte[] bytes = new byte[5];

            //读取到的字符个数
            int read = fileInputStream.read(bytes);

            //遍历字节数组
            for (byte aByte : bytes) {
                System.out.println(aByte);
            }

            read = fileInputStream.read(bytes);

            System.out.println(read);

            System.out.println("---------------------------------");

            //遍历字节数组
            for (byte aByte : bytes) {
                System.out.println(aByte);
            }

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fileInputStream != null){
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

![](../_assets/87c22383_1747536391461-efd44f75-41b7-4339-a8bc-4ccfb0ed83fe.png)

数字2是因为打印了5个 第二次7个只剩余2个

创建数组并用while循环，将遍历到的ascll码转换成对应的字符

```java
package com.io;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileInputStreamTest3 {

    public static void main(String[] args) {

        FileInputStream fileInputStream = null;

        try {
            //创建字节输入流
            fileInputStream = new FileInputStream("C:\\temp\\abc.txt");

            //创建byte数组
            byte[] bytes = new byte[5];

            //            //将字节数组中所有的ascll转换成对应的字符
            //            String s = new String(bytes,0,read);
            //            System.out.println(s);
            //
            //            read = fileInputStream.read(bytes);
            //
            //            String s2 = new String(bytes,0,read);
            //            System.out.println(s2);

            //读取数据 返回值是读取字符的个数，我们一般在转换的时候：读取多少个字符，就转换多少个字符
            int num = fileInputStream.read(bytes);
            //while循环
            while (num != -1){

                //将ascll码转换为对应的字符 长度为从0到num
                String str = new String(bytes,0,num);
                System.out.print(str);
                num = fileInputStream.read(bytes);

            }

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fileInputStream != null){
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

![](../_assets/36e74c8a_1747536513109-cbe273a5-39e1-4d6b-a8a2-995296985359.png)

代码更简便好看一点

```java
package com.io;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FileInputStreamTest4 {

    public static void main(String[] args) {

        FileInputStream fileInputStream = null;

        try {
            //创建字节输入流
            fileInputStream = new FileInputStream("C:\\temp\\abc.txt");

            //创建byte数组
            byte[] bytes = new byte[5];

            //读取数据 返回值是读取字符的个数，我们一般在转换的时候：读取多少个字符，就转换多少个字符
            int num = 0;
            //将变量放在while的循环条件里面
            while ((num = fileInputStream.read(bytes)) != -1){
                //将ascll码转换为对应的字符
                String str = new String(bytes,0,num);
                System.out.print(str);
            }

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fileInputStream != null){
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

![](../_assets/2477733d_1747536526201-f4f07c6b-e295-4759-b759-65d9c0e93103.png)
