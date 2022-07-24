# 常用数据结构

## StringBuilder、StringBuffer、String

String 和 StringBuffer、StringBuilder 的区别在于 <mark style="color:orange;">String 声明的是不可变的对象</mark>，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 <mark style="color:orange;">StringBuffer、StringBuilder 可以在原有对象的基础上进行操作</mark>，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，<mark style="color:orange;">StringBuffer 是线程安全的</mark>，<mark style="color:orange;">而 StringBuilder 是非线程安全的</mark>，但 <mark style="color:orange;">StringBuilder 的性能却高于 StringBuffe</mark>r，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

## HashMap

HashMap概述：HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。&#x20;

HashMap的数据结构：在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即<mark style="color:orange;">数组和链表的结合体</mark>。

当我们往Hashmap中put元素时,首先根据key的hashcode重新计算hash值,根绝hash值得到这个元素在数组中的位置(下标),如果该数组在该位置上已经存放了其他元素,那么在这个位置上的元素将以链表的形式存放,新加入的放在链头,最先加入的放入链尾.如果数组中该位置没有元素,就直接将该元素放到数组的该位置上。

需要注意<mark style="color:orange;">Jdk 1.8中对HashMap的实现做了优化,当链表中的节点数据超过八个之后,该链表会转为红黑树来提高查询效率,从原来的O(n)到O(logn)</mark>

## ConcurrentHashMap

## TreeMap

