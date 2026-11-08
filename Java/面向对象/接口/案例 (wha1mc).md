---
title: "案例"
yuque_slug: "wha1mcsdiaipilw1"
yuque_id: 216230629
yuque_updated: "2026-10-14T04:52:54.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/wha1mcsdiaipilw1"
---
# 案例：

```plain
案例：
 -创建一个用户登录接口
   UserLogin
      -抽象方法 login
      - 效验用户名和密码，如果用户名是jack、密码是123456，方法返回登录成功
      -用匿名内部类完成
```

```java
package com.eagles.demo;

public interface UserLogin {

    String login(String name,String keyword);

}
```

```java
package com.eagles.demo;

public class Test {

    public static void main(String[] args) {

        //多态
        UserLogin userLogin = new UserLogin() {
            @Override
            public String login(String name, String keyword) {

                if(name.equals("jack")&&keyword.equals("123456")){
                    return "登录成功";

                }
                return "登录失败";
            }
        };

        //接收           引用                赋值
        String login = userLogin.login("jack","123456");

        System.out.println("是否登录成功："+login);
    }
}
```
