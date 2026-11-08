---
title: "Static关键字"
yuque_slug: "hluo9tdtdas62yps"
yuque_id: 215778957
yuque_updated: "2026-10-14T04:52:54.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/hluo9tdtdas62yps"
---
# Static：

static修饰的属性，会在类被加载的时候，直接放入到方法区中，被所有类的对象共享，它是先于对象存在的，无论有没有对象，都已经有他了！

static可以用来修饰：属性、方法、代码块、内部类

属性：是否使用static修饰，又分为：静态属性 和 非静态属性（实例变量）

#### 实例变量：

创建类的多个对象，每个对象都独立的拥有了一套类中的非静态属性。

当修改其中一个非静态属性时，不会导致其他对象中同样的属性值修饰。

#### 静态变量:

创建类的多个对象，多个对象共享一个静态变量。

当通过静态变量去修饰某一个变量时，会导致其他对象调用此静态变量时，是修改过的。

#### static修饰属性的其他说明：

1.静态变量随着类的加载而加载。可以通过"类.静态变量"的方式进行调用

2.静态变量的加载要早于对象的创建。

3.由于类只会加载一此，则静态变量在内存中也只会存在一次。存在方法区的静态域中。

因为static修饰的内容会放到方法区，占用空间！所以一般常用的那些变量或者方法，才采用static修饰！

#### 注意：

在静态的方法内，不能采用\*\*this\*\*关键字。\*\*super\*\*关键字。

# 总结：

- 静态变量可以直接用类名访问，也称类变量
- 静态变量（或方法）对于类，所有对象（实例）共享
- 静态变量可以直接调用，但是非静态变量不可以直接调用
- 静态代码块优先被加载，并且只加载一次！！

```plain
/*
  * Static是静态关键字
  * -属性   静态变量
  * -方法   静态方法
  * -代码块  也会随着类的加载而加载，静态代码块的执行的优先级要高于主函数
  * -内部类

eg：private static int age；
现将java源代码编译成字节码文件，在编译字节码文件之前java底层会去调用一个类加载器.class nodel，他要找到你对应的源文件再什么地方，
 然后再去调用javac.exe，将源文件生成对应的自己诶嘛文件他会随着类的加载而加载，类的加载就是生成字节码文件的时候，这个时候他就会把age
 这一份空间放在方法区中
 */
```

低级可以调用高级，调用static类级

```java
public   void  getmethod(){
    System.out.println("这是静态方法");
    getmethod02();
    //也可以拿着类直接去点，因为是同一级别
    StaticTest.getmethod02();
}

public static void getmethod02(){ //类级
    System.out.println("这是个实例方法！");

}
```

```plain
//静态代码块 也会随着类的加载而加载，静态代码块的执行的优先级要高于主函数
static {
     System.out.println("静态代码块执行力~");
 }

 //通过static来修饰属性
 //内存开辟的时机：类加载的时候，静态变量的内存空间，无论什么情况下，只会开辟一次
 private static String name;//静态变量，内存是开辟在方法区中的
                            // (静态变量，会被所有的变量所共享，因为是类级别的)
```

```java
package com.eagles.test;

public class StaticTest {

    //静态代码块 也会随着类的加载而加载，静态代码块的执行的优先级要高于主函数（放在末尾也是优先运行）
    static {
        System.out.println("静态代码块执行了~");
    }

    //通过static来修饰属性
    //内存开辟的时机：类加载的时候，静态变量的内存空间，无论什么情况下，只会开辟一次
    private  String name;//属性
    private static  int age;  // (静态变量，内存是开辟在方法区中的,会被所有的变量所共享，因为是类级别的)

    public static void main(String[] args) {
        //创建当前类的对象
        StaticTest staticTest =  new StaticTest();
        staticTest.getmethod();
        //静态方法可以直接用类名去.方法名称
        staticTest.getmethod02();
    }

    //方法压栈，当你的方法被调用的时候，那么就会在栈中开辟空间（无论是静态方法还是实例方法都一样）
    public   void  getmethod(){
        System.out.println("这是静态方法");
        getmethod02();
    }
    //static 类级别的方法
    public static void getmethod02(){
        System.out.println("这是个实例方法！");

    }

}
```
