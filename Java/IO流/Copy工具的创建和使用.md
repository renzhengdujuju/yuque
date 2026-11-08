---
title: "Copy工具的创建和使用"
yuque_slug: "mug5qio78r8c9gmg"
yuque_id: 220073580
yuque_updated: "2026-10-14T04:53:25.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/mug5qio78r8c9gmg"
---
```java
package com.utils;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyUtils {

    /*构造方法私有化，不可以被实例化对象*/ 
    private CopyUtils(){

    }
    /*
    互联网上信息的最小计量单位就是字节，所以通过字节流可以将图片、视频、音频等进行传输，写CopyUtils就是写一个工具使得
    在任何复制的情况下都可以使用该工具
     */

    /**
     *
     * @param inputPath 输入文件的路径
     * @param outputPath 输出文件的路径
     */
    public static void copyFile(String inputPath,String outputPath){

        FileInputStream fileInputStream = null;

        FileOutputStream fileOutputStream = null;

        try {

            //创建输入流
            fileInputStream = new FileInputStream(inputPath);
            //创建输出流
            fileOutputStream = new FileOutputStream(outputPath);

            //创建字节数组
            byte[] bytes = new byte[1024 * 1024];

            //开始读取文件中的数据
            int read = 0;

            while ((read = fileInputStream.read(bytes)) != -1){

                //String s = new String(bytes,0,read);
                //开始写数据 一边读 一边写
                fileOutputStream.write(bytes,0,read);

            }

            //刷新管道，不然可能会造成数据的错乱
            fileOutputStream.flush();

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }finally {

            if (fileInputStream != null){
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }

            }

        }

        if (fileOutputStream != null){

            try {
                fileOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }

}
```

```java
package com.io;

import com.utils.CopyUtils;	

public class CopyTest02 {

    public static void main(String[] args) {

        //调用工具类中copy的方法  写如读入地址和Copy地址
        CopyUtils.copyFile("C:\\temp\\01.Python环境安装.mp4","C:\\temp\\01.Python环境安装-copy.mp4");

    }

}
```

此处没有视频所以没有图片说明Copy过程
