---
title: "HashMap的特征及遍历方式"
yuque_slug: "emzvoccxiqkcetgd"
yuque_id: 219500626
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/emzvoccxiqkcetgd"
---
```java
package com.map;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class HashMapTest01 {
    public static void main(String[] args) {
        //创建一个HashMap的集合对象
        HashMap<String, Object> HashMap = new HashMap<>();

        //储存元素
        HashMap.put("name", "jack");
        HashMap.put("age", 20);
        HashMap.put("address", "南京市");

        //遍历集合

        //1.获取该集合所有key的集合  再通过get方法将value给遍历出来
        Set<String> keys = HashMap.keySet();

        for (String key : keys) {
            System.out.println(key +"==>"+ HashMap.get(key));
        }

        //2.存在一个遍历集合的方法entryset  getkey和getvalue直接获取到数据
        Set<Map.Entry<String, Object>> entries = HashMap.entrySet();
        for (Map.Entry<String, Object> entry : entries) {
            System.out.println(entry.getKey() +"==>" + entry.getValue());
        }

    }
}
```

![](../_assets/eccfab22_1747186838471-91a641f6-849a-4ee6-9a0a-429323b79cc7.png)
