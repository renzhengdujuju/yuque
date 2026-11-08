---
title: "ArrayList的使用"
yuque_slug: "gobhfmuma757i2rr"
yuque_id: 218259234
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/gobhfmuma757i2rr"
---
![](../_assets/8ad071b4_1746456016582-b19b0670-684f-4c3d-a05e-45645409c3a2.png)  
ArratList封装的是数组的数据结构，LinkList底层封装的是链表的数据结构，HashEet封装的是哈希散列，TreeSet封装的是二叉数，每一个集合的底层都会封装对应的数据结构，也就是说他们存储的机制是不一样的。

数组查询效率高，但是随机增删效率会比较低

链表查询效率低，但是随机增删效率会比较高

![](../_assets/ba5d4d82_1746457045119-212ec94c-0400-4fb8-85b6-b60cd84a5647.png)

```java
package com.collection;

import java.util.ArrayList;

public class ArrayListTest01 {

    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();//默认情况下arraylist里面也是存放Objecct类型的数据
        arrayList.add("hello");
        arrayList.add(100);
        //可以根据上标索引来获取对应的元素
        Object obj = arrayList.get(0);//从0开始的，下标为0的是第一个元素
        System.out.println(obj);
    }
}
```

![](../_assets/172e0723_1746457874722-ce23f993-bf82-403f-9d6b-a8c49c7f8ea2.png)

打印的是第一个数据，是hello

```java
package com.collection;

import java.util.ArrayList;
/*
arraylist是一个有序可重复的集合，有序体现在存入元素和取出的顺序是一样的
*/
public class ArrayListTest01 {

    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        //默认情况下arraylist里面也是存放Objecct类型的数据
        //底层封装的是数组，数组就是查询效率高
        arrayList.add("hello");
        arrayList.add(100);
        arrayList.add(3.14);
        //可以根据上标索引来获取对应的元素
        // Object obj = arrayList.get(0);//从0开始的，下标为0的是第一个元素
        // System.out.println(obj);
        //可以用传统方法进行一个遍历
        for (int i = 0; i < arrayList.size() ; i++) {
            //想遍历每一个元素
            System.out.println(arrayList.get(i));

        }
    }
}
```

![](../_assets/752957df_1746458457406-83847420-f2be-4223-b4e5-4e14209907bf.png)

Vector是一个线程安全版的arraylist集合，效率低，原因是每次都会加一个关键字synchonized（同步的关键字），在多线程的情况下只能保证有一个线程去访问这个方法
