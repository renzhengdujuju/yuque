---
title: "Collection集合中的常用方法"
yuque_slug: "toab8gxba3k0ndo2"
yuque_id: 216417150
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/toab8gxba3k0ndo2"
---
![](../_assets/48cc698e_1746447248987-693f06e9-26e9-4771-8ac4-f84cc90ceaf1.png)

最大的父接口Collection，下面有两个子接口List、Set。（绿色的都是接口）

![](../_assets/c9f75f05_1746447422511-c6922ee6-9c33-42ce-b792-bc4d3b5f4750.png)

上面显示的流程图只是子接口里面的一部分，且还有一个父接口Iterable（迭代器接口）。在collection上面，算collection的父类 。

这里面所包含的方法：

![](../_assets/daa266b4_1746451654887-4682a4ac-3224-4fd3-8797-9fe4fa198854.png)

Add方法：

```java
package com.collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest01 {
    //接口不能实例化对象，所以去找一个他的子类，运用多态去进行实例化对象
    //创建一个集合对象
    public static void main(String[] args) {
        Collection  collection = new  ArrayList();

        //往集合中去添加元素
        collection.add(100);  //集合里面只能放引用数据类型，可以存放基本数据类型的原因是，存在自动装箱
        collection.add(true);  //自动装箱
        collection.add(new Object()); //自动装箱
        collection.add("3.14"); //自动装箱
        System.out.println("集合中一共有多少个元素："+ collection.size());
    }
}
```

![](../_assets/ceb3b75e_1746449044963-8dc3f641-badd-4577-820d-70bf324f962c.png)

Claer方法：

```java
package com.collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest01 {
    //接口不能实例化对象，所以去找一个他的子类，运用多态去进行实例化对象
    //创建一个集合对象
    public static void main(String[] args) {
        Collection  collection = new  ArrayList();

        //往集合中去添加元素
        collection.add(100);  //集合里面只能放引用数据类型，可以存放基本数据类型的原因是，存在自动装箱
        collection.add(true);  //自动装箱
        collection.add(new Object()); //自动装箱
        collection.add("3.14"); //自动装箱
        System.out.println("集合中一共有多少个元素："+ collection.size());

        //清除集合中的元素
        collection.clear();
        System.out.println("集合中一共有多少个元素："+ collection.size());
    }
}
```

![](../_assets/81d92432_1746450377022-31fa02ef-62f4-4e7d-9684-9a5fc731909e.png)

Contains方法：

```java
package com.collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest01 {
    //接口不能实例化对象，所以去找一个他的子类，运用多态去进行实例化对象
    //创建一个集合对象
    public static void main(String[] args) {
        Collection  collection = new  ArrayList();

        //往集合中去添加元素
        collection.add(100);  //集合里面只能放引用数据类型，可以存放基本数据类型的原因是，存在自动装箱
        collection.add(true);  //自动装箱
        collection.add(new Object()); //自动装箱
        collection.add("3.14"); //自动装箱
        System.out.println("集合中一共有多少个元素："+ collection.size());

        //判断集合中是否存在某一个元素
        boolean contains = collection.contains(100);
        System.out.println("是否包含："+contains);
    }
}
```

![](../_assets/88be0e95_1746450881674-49ce2f4a-f9c4-4d40-b13f-aa85b9848f3a.png)

```java
boolean contains = collection.contains(101);

boolean contains = collection.contains(new Object());

在后面写这两个都会显示false，原因是因为该元素里面没有101元素，而new Object()
也显示fales是因为new Object()是因为开辟了新的空间所对应的内存地址不一样。
```

Remove方法：

```java
package com.collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest01 {
    //接口不能实例化对象，所以去找一个他的子类，运用多态去进行实例化对象
    //创建一个集合对象
    public static void main(String[] args) {
        Collection  collection = new  ArrayList();

        //往集合中去添加元素
        collection.add(100);  //集合里面只能放引用数据类型，可以存放基本数据类型的原因是，存在自动装箱
        collection.add(true);  //自动装箱
        collection.add(new Object()); //自动装箱
        collection.add("3.14"); //自动装箱
        System.out.println("集合中一共有多少个元素："+ collection.size());

        //判断集合中是否存在某一个元素
        boolean contains = collection.contains(101);
        System.out.println("是否包含："+contains);

        boolean contains02 = collection.contains(new Object());
        System.out.println("是否包含："+contains02);

        //删除元素
        boolean remove = collection.remove(100);
        System.out.println("是否删除成功："+remove);
        System.out.println("集合中一共有多少个元素："+ collection.size());
    }
}
```

![](../_assets/d051bc13_1746451984777-a03ea06e-985e-4e1d-a936-559fc089fa4c.png)

```java
package com.collection;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionTest01 {
    //接口不能实例化对象，所以去找一个他的子类，运用多态去进行实例化对象
    //创建一个集合对象
    public static void main(String[] args) {
        Collection  collection = new  ArrayList();

        //往集合中去添加元素
        collection.add(100);  //集合里面只能放引用数据类型，可以存放基本数据类型的原因是，存在自动装箱
        collection.add(true);  //自动装箱
        collection.add(new Object()); //自动装箱
        collection.add("3.14"); //自动装箱
        System.out.println("集合中一共有多少个元素："+ collection.size());

        //将集合转成数组
        //会自动返回成一个Object的数组,Object可以去接收所有的元素
        Object[] objects = collection.toArray();
        //用加强for循环来对他进行一个遍历
        for (Object obj:objects){
            System.out.println(obj);
        }

    }
}
```

![](../_assets/be194760_1746452591538-cb941a77-c0f1-43bb-b5c9-e9e7de4578f5.png)java.lang.Object@4554617c是new Object（）的内存地址
