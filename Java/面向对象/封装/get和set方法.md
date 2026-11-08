---
title: "get和set方法"
yuque_slug: "wkuqftymv8le1q9f"
yuque_id: 215647760
yuque_updated: "2026-10-14T04:52:54.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/wkuqftymv8le1q9f"
---
# get和set方法

- 将某些东西隐藏，然后提供相应的方式进行获取

。我们程序设计要追求“高内聚，低耦合”。高内聚就是类的内部数据细节由自己完成，不允许外部干涉；低耦合就是仅暴露少量的方法给外部使用。

- 封装（数据的隐藏）

。通常，应禁止直接访问一个对象中数据的实际表达，而应该通过操作接口来访问，称为信息隐蔽。

- 属性私有（get/set）

```java
package com.eagles.pojo;

/*面向对象需要封装的方法
    -get（）获取；set（设置）方法
 */
public class User {
    private int age;
    private String name;
    //加了public可以在当前工程的任意路径都可以访问
    //private 私有的权限修饰符（只有在当前类才能被访问）

    /*
    get 方法（get必须要返回值）
     */
    public String getName(){

        //this 当前类附引用或者实例  实例只能去点成员变量
        return  this.name;//（get必须要返回值）

    }
   /*
   set 方法
    */
    //void此处不需要给方法的调用处返回任何的数据，不像前面要返回name
    public void setName(String name){
        this.name=name;//一个是局部变量（String name里面的name定义在参数里面）
                       // 一个是成员变量（this.name里面的name  类.成员变量）
                       //get用法是为了将私有的属性可以被访问到，set是为了设置属性的值
    }
}
     

    /*public int getAge（）{
         if （this.age >25 ）{
             return 18；
         }
         return age；
     }*/
   也可以在里面直接进行条件选择
```

```java
package com.eagles.test;

import com.eagles.pojo.User;

public class UerTest {
    public static void main(String[] args) {

        //创建user对象
        User user = new User();

        String name = user.getName();
        System.out.println(name);

        user.setName("Tom");

        String name1 = user.getName();
        System.out.println(name1);

        //给age赋值
        user.setAge(30);
        //打印方法一：直接打印
        System.out.println(user.getAge());
        //打印方法二：获取值再打印
        int age = user.getAge();
        System.out.println(age);

        //给address赋值
        user.setAddress("南京市");
        System.out.println(user.getAddress());

    }
}
```

```java
public class PersonTest {
    public static void main(String[] args) {
        Person person = new Person();

        //使用set方法 设置值
        person。setName("herb");
        person.setAge(30);
        person.setHeight(100);
        perosn.setWeight(100);

        //通过get方法 获取值
        System.out.println（person.getName()）;
        System.out.println（person.getAge()）;
        System.out.println（person.getHeight()）;
        System.out.println（person.getWeight()）;
    }
}  
```

# 作用

。提高程序的安全性，保护数据

。隐藏代码的实现细节

。统一接口

。系统可维护性增强

```java
//有参构造
     public User（String name、int age）{
       this.name = name;
       this.age = age;

       }
```

```java
  //通过有参构造创建对象
      User user = new User（）；
      new User（name；“张三”，age；30）
      即对有参构造进行赋值了
 //打印
  System.out.println(user.getName());//直接就通过getName获取值
  System.out.println(user.getAge());
```

# 重载

方法名称相同但是参数列表不同。

当在类中定义看任意一个构造函数，默认的构造函数将会被覆盖掉

# 构造方法

-作用：用来创建对象

-特点；可以用来创建对象、方法没有返回值啥类型

-语法：

public 类名（） {

}

public 类名（int x ，double y ） {

this.x = x;

this.y = y;

}

-有参构造

-含有参数的构造方法，在创建对象的同时，可以给类中的属性进行赋值

-无参构造

-在一个类中，会默认存在，直接创建对象，而不会给类中的属性赋值

public class User(){

public User(){ //无参构造，默认会存在，其他函数调用User时都是调用这个默认存在的无参构造，除非构造 新的函数将类的无参构造覆盖

}

//在其他文件创建User这个对象的时候，都是调用的这个默认的无参构造，

每次创建无参构造的时候，这个函数都会被触发

}

```java
package com.eagles.pojo;

public class Book {
    private String title;
    private String author;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title= title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;

    }
    public Book(String title,String author){
        this.title = title;
        this.author = author;
    }
}
```
