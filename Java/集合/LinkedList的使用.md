---
title: "LinkedList的使用"
yuque_slug: "kwasogx54ehxo63n"
yuque_id: 218263143
yuque_updated: "2026-10-14T04:53:15.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/kwasogx54ehxo63n"
---
![](../_assets/d98e6c78_1746626807309-c85239e1-a99f-4375-a90c-59ad36aa34dd.png)

Node就是节点，每一个Node有一个首元素和一个尾元素，每一个节点都会存储相应的数据data和下一个节点的内存地址![](../_assets/dcad5a9f_1746627236514-5c1acd58-b3b9-4d8f-809b-3570d393ba77.png)

最后一个节点里面只存储了数据，没有存储下一个节点的内存地址。

查询效率低的原因是：你要在链表里面去进行查询的话，每次查询的时候都会把每一个节点都过一遍。

随机增删效率高的原因是：删除某一个元素只需要删除节点就好了，如何删除的呢，拿上图演示：直接删除节点，然后把最后一个的内存地址赋值给删除前的那个节点即可![](../_assets/c9c210f8_1746627588103-2444dfb5-47aa-43be-8321-27f9416923ad.png)
