---
title: "Set的基本用法"
yuque_slug: "xfl0tuxm5mxhqexi"
yuque_id: 220175962
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/xfl0tuxm5mxhqexi"
---
![](../_assets/dafbd6f8_1747611333900-dc1a1280-23c2-4b73-99ce-c91e41b4dd6c.png)

Set：无序不可重复

# HashSet

```java
package com.map;

import java.util.HashSet;

public class HashSetTest01 {
    public static void main(String[] args) {
        HashSet<String> hashSet = new HashSet<>();
        hashSet.add("hello");
        hashSet.add("world");
        hashSet.add("java");
        hashSet.add("java");

        for (String s : hashSet) {
            System.out.println(s);
        }
    }
}
```

两个"java"字符只打印了一个

![](../_assets/a738c8c3_1747611792933-31bc2bc1-46c4-4607-88a2-8812af049dcc.png)

按住Alt点进去，你会发现他会把数据用字母e来代替，e放在map里面的key的位置

![](../_assets/37bb3e7d_1747611906338-d5b78c31-c275-4f15-8f10-edd649a49108.png)

![](../_assets/596036a0_1747612104737-9042a36c-887a-4d47-b548-cccce11afc52.png)

```java
package com.map;

import java.util.HashSet;
import java.util.Iterator;

public class HashSetTest01 {
    public static void main(String[] args) {
        //创建一个map集合
        //HashSet底层是封装的是HashMap，我们添加数据，实际上数据存储在map集合的key部分
        HashSet<String> hashSet = new HashSet<>(); //无序，不可重复
        hashSet.add("hello");
        hashSet.add("world");
        hashSet.add("java");
        hashSet.add("java");
        System.out.println("------------------------------for循环遍历----------------------------------");
        for (String s : hashSet) {
            System.out.println(s);
        }
        System.out.println("------------------------------迭代器遍历----------------------------------");
        Iterator<String> iterator = hashSet.iterator();
        while (iterator.hasNext()){
            String data = iterator.next();
            System.out.println(data);
        }
    }
}
```

![](../_assets/3b994f08_1747612458712-4b5a4409-7ebd-44f5-9b71-78afca4fec99.png)

# TreeSet

![](../_assets/79b81d5f_1747613653252-85a5b764-8b42-4b44-8f85-4a9356ff84ae.png) ![](../_assets/c0b7808e_1747613668642-59711c00-80fb-4b60-a16f-b1411484797d.png)![](../_assets/929eb164_1747613680365-aea8739d-e9eb-4914-9f1a-8644f26e0d4b.png)

![](../_assets/bf36837c_1747613691762-52923fbf-8661-4461-a7df-c4c4b789d750.png)

输入的数据e间接的继承到了Map下面，Integer里面继承了comparable方法，其原理和HashSet一样，只是继承的Map子类不一样

```java
package com.map;

import java.util.TreeSet;

public class TreeSetTest {
    public static void main(String[] args) {
        //创建TreeMap集合
        TreeSet<Integer> treeSet = new TreeSet<>();

        //往集合中添加数据
        treeSet.add(100);
        treeSet.add(200);
        treeSet.add(500);
        treeSet.add(350);
        treeSet.add(150);

        //通过for循环遍历
        System.out.println("-------------------------------------------------");
        for (Integer data : treeSet) {
            System.out.println(data);
        }
    }
}
```

![](../_assets/2faa19ab_1747613859861-0b26492f-a7f8-4b32-ae19-63f5e1832b48.png)
