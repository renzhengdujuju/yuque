---
title: "案例分析-角色回合制PK"
yuque_slug: "kvmtwztl6hnfs4va"
yuque_id: 216029733
yuque_updated: "2026-10-14T04:52:54.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/kvmtwztl6hnfs4va"
---
需求1：通过面向对象模拟两个游戏角色回合制PK的场景

```java
package com.eagles.gamecharacter;

public class Gamecharacter {
    //姓名
    private String name;
    //职业
    private  String job;
    //血量
    private  double bleed;
    //技能
    private String skill;
    //攻击力
    private double ATK;

    public Gamecharacter(String name, String job, double bleed, String skill, double ATK) {
        this.name = name;
        this.job = job;
        this.bleed = bleed;
        this.skill = skill;
        this.ATK = ATK;
    }

    public Gamecharacter() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getJob() {
        return job;
    }

    public void setJob(String job) {
        this.job = job;
    }

    public double getBleed() {
        return bleed;
    }

    public void setBleed(double bleed) {
        this.bleed = bleed;
    }

    public String getSkill() {
        return skill;
    }

    public void setSkill(String skill) {
        this.skill = skill;
    }

    public double getATK() {
        return ATK;
    }

    public void setATK(double ATK) {
        this.ATK = ATK;
    }
    /*
    tostring是一个重写object类的一个方法
    @Override
    public String toString() {
       //打印的是一个哈希值（当前对象的一个内存地址）， return super.toString();
    }
    */

    //它可以打印你当前类里面的属性，和属性对应的值是什么。（重写tostring）
    @Override
    public String toString() {
        return "gamecharacter{" +
        "name='" + name + '\'' +
        ", job='" + job + '\'' +
        ", bleed=" + bleed +
        ", skill='" + skill + '\'' +
        ", ATK=" + ATK +
        '}';
    }
}
```

```java
package com.eagles.gamecharacter;
//场景类
public class WinandLose {
    //游戏角色1
    private Gamecharacter gamecharacter1;
    //游戏角色2
    private Gamecharacter gamecharacter2;

    /*如果想创建当前类的实例，必须通过有参构造来创建对象，换句话来说，如果想创建当前类的实例，必须要把两个游戏对象要传进来才可以
    要求在创建对象的时候必须要传什么进来就通过这种方式*/
    public WinandLose(Gamecharacter gamecharacter1, Gamecharacter gamecharacter2) {
        this.gamecharacter1 = gamecharacter1;
        this.gamecharacter2 = gamecharacter2;
    }

    public void PK(){
        while (true){
            if (gamecharacter1.getBleed() <= 0 ||gamecharacter2.getBleed() <= 0){
                break;
            }
            //打印分割线好看一点
            System.out.println("------------------------------------------------------------------------");

            System.out.println(gamecharacter1.getName()+"发起进攻，使出了"+gamecharacter1.getSkill()+"对"+gamecharacter2.getName()
                               +"造成了"+gamecharacter1.getATK()+"点伤害！");
            //扣除每回合攻击完之后，对应的血量
            gamecharacter2.setBleed(gamecharacter2.getBleed() - gamecharacter1.getATK());

            System.out.println(gamecharacter2.getName()+"发起进攻，使出了"+gamecharacter2.getSkill()+"对"+gamecharacter1.getName()
                               +"造成了"+gamecharacter2.getATK()+"点伤害！");
            //扣除每回合攻击完之后，对应的血量
            gamecharacter1.setBleed(gamecharacter1.getBleed() - gamecharacter2.getATK());

            //加一个延时，不想直接看到结果，想要看到一个具体的过程   睡眠方法
            try {
                Thread.sleep(500);//线程类 单位是毫秒即0.5秒
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

        }
        //判断谁的血量高谁就获胜
        if (gamecharacter1.getBleed()> gamecharacter2.getBleed()){
            System.out.println(gamecharacter1.getName()+"KO了"+gamecharacter2.getName());
        }else {
            System.out.println(gamecharacter2.getName()+"KO了"+gamecharacter1.getName());
        }

    }
}
```

```java
package com.eagles.gamecharacter;

public class GamecharacterTest {

    public static void main(String[] args) {

        //创建游戏角色对象
        //无参构造还得再使用set方法去赋值
        Gamecharacter gamecharacter1 = new Gamecharacter();
        gamecharacter1.setName("李白");
        gamecharacter1.setBleed(5000);
        gamecharacter1.setATK(200.0);
        gamecharacter1.setJob("刺客");
        gamecharacter1.setSkill("青莲剑歌");
        //有参考构造创建，在创建对象的同时可以给类里面的属性赋值
        Gamecharacter gamecharacter2 = new Gamecharacter("百里守约","射手",3000,"打水机",300.0);

        //创建场景类对象
        WinandLose winandLose = new WinandLose(gamecharacter1,gamecharacter2);

        winandLose.PK();
    }

}
```
