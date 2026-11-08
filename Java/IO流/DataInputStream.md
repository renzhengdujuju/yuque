---
title: "DataInputStream"
yuque_slug: "lfllob6v31kl8cbl"
yuque_id: 220100426
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/lfllob6v31kl8cbl"
---
```java
package com.io;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;

/**
 * 数据流 可以对数据进行加密
 *  -跟据对应的数据类型来存取数据 也要根据数据类型来取数据
 *  -写入的顺序跟取出的顺序必须一样，不然会造成乱码
 */

public class DataInputStreamTest {

    public static void main(String[] args) throws Exception {

        //创建数据流输入对象
        DataInputStream dataInputStream = new DataInputStream(new FileInputStream("data2.txt"));

        //根据数据类型来读取数据
        double readDouble = dataInputStream.readDouble();
        int readInt = dataInputStream.readInt();
        boolean readBoolean = dataInputStream.readBoolean();
        char readChar = dataInputStream.readChar();

        System.out.println(readDouble);
        System.out.println(readInt);
        System.out.println(readBoolean);
        System.out.println(readChar);

        dataInputStream.close();

    }

}

class DataOutputStreamTest{

    public static void main(String[] args) throws Exception {

        //创建数据流输出流对象
        DataOutputStream dataOutputStream = new DataOutputStream(new FileOutputStream("data2.txt"));

        //根据数据类型来写入数据
        dataOutputStream.writeInt(100);
        dataOutputStream.writeBoolean(false);
        dataOutputStream.writeDouble(3.14);
        dataOutputStream.writeChar('中');

        dataOutputStream.flush();
        dataOutputStream.close();

    }

}
```

![](../_assets/2700006d_1747556556815-2fcb0d48-13ff-4b00-bda1-9d8f78476b2d.png)

如果输入顺序和输出顺序不一样的话，会出现乱码，比如：

![](../_assets/991c960b_1747556711050-60ee473d-d167-4b31-a039-c7e5de72daff.png)

再次运行程序  
![](../_assets/e35fd50c_1747556743102-aa700946-c85c-4461-9258-3747d4f88c1f.png)

程序无法正常运行
