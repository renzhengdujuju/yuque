---
title: "Properties"
yuque_slug: "vox8341vpm8ac7vb"
yuque_id: 220177621
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/vox8341vpm8ac7vb"
---
创建一个配置文件 ps:(文件后缀 .propertise)

```java
db.username=root
db.password=123456
db.url=jdbc:mysql://127.0.0.1/test
db.driver=com.cj.jdbc.Driver
```

```java
package com.demo.map;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;

public class PropertiesDemo {

    public static void main(String[] args) {

        //该集合可以读取外部的配置文件
        Properties properties = new Properties();
        FileReader fileReader = null;

        try {
            //创建IO流对象
            fileReader = new FileReader("C\\temp\\db.properties");
            //System.out.println(fileReader);

            //结合流对象读取配置文件
            properties.load(fileReader);

            String username = properties.getProperty("db.username");
            System.out.println(username);

            String password = properties.getProperty("db.password");
            System.out.println(password);

            String url = properties.getProperty("db.url");
            System.out.println(url);

            String driver = properties.getProperty("db.driver");
            System.out.println(driver);

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            if (fileReader != null){
                try {
                    fileReader.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }

    }

}
```

会成功打印
