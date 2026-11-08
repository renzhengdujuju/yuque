---
title: "java程序的执行原理"
yuque_slug: "aggg7z0p790kn6d7"
yuque_id: 240315780
yuque_updated: "2026-10-14T04:52:26.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/aggg7z0p790kn6d7"
---
![](../_assets/e5c4ec4d_1760343649417-80f5e3ea-3a18-4cb7-a463-5b70c4d24764.png)

java的执行原理：

-java源代码，计算机不能直接执行

-【javac 文件名 】指令执行之后，在底层会调用类加载器classLoader

-类加载器会根据你提供的文件名找到对应的java源文件

-会生成对应的字节码文件.class

-执行【java 类名】指令，jvm会根据字节码文件生成二进制文件，再跟计算机的底层系统交互，最终将hello world 打印在控制台上面
