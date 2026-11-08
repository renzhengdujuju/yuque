---
title: "Final关键字"
yuque_slug: "uzmx36grhrlpupp1"
yuque_id: 215761034
yuque_updated: "2026-10-14T04:52:54.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/uzmx36grhrlpupp1"
---
# Final：

在java里面使用final关键字可以实现如下功能：定义不能够被继承的类，不能够被覆写的方法，常量。

```plain
/*
   final 可以修饰类、变量、方法
     -被final修饰的类，不能被子类继承
     -被final修饰的变量、不能被重新赋值（被final修饰的属性，如果是基本数据类型，一定在定义变量的时候就初始化；如果是引用数据类
型，在程序运行过程中，变量的引用不能指向别的内存地址）
     -被final修饰的方法、不能被子类重写
    */
```

1.被final修饰的类，不能被子类继承：Myclass报错

```java
package com.eagles.pojo;

public final class MyClass {
}
class Test02 extends MyClass{  //创建了一个Test02这个类
    public static void main(String[] args) {

    }
}
```

2.被final修饰的变量、不能被重新赋值：变量i会报错；obj报错

```java
package com.eagles.pojo;

public final class MyClass {
}
class Test02 {  //创建了一个Test02这个类
    public static void main(String[] args) {
        //int i = 100;
        //i = 101;  
        //正常情况下变量可以再一次被赋值，但是如果加入final关键字

        final  int i = 100;
        i = 101;//变量i会报错
    }
}
```

```java
package com.eagles.pojo;

public final class MyClass {
}
class Test02 {  //创建了一个Test02这个类
    public static void main(String[] args) {
        final Object obj = new Object(); //加上final之后obj将不能在指向别的内存地址，即不能再被赋值
        obj = new Object();  //obj报错
    }

}
```

3.被final修饰的方法、不能被子类重写：Method报错

```java
package com.eagles.pojo;

public  class MyClass {
    public final void Method(){

    }
}
class Test02 extends MyClass {  //创建了一个Test02这个类
    public  void  Method(){ //Method报错

    }

} 
//有同名方法+final关键字不可以重写  
```

用final关键字能不能修饰抽象类？不可以，因为抽象类的作用就是用来被子类继承的，但是被final修饰

的类是无法被子类继承的，final和abstract是不能同时出现的
