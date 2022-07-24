# 常用数据结构

## StringBuilder、StringBuffer、String

String 和 StringBuffer、StringBuilder 的区别在于 <mark style="color:orange;">String 声明的是不可变的对象</mark>，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 <mark style="color:orange;">StringBuffer、StringBuilder 可以在原有对象的基础上进行操作</mark>，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，<mark style="color:orange;">StringBuffer 是线程安全的</mark>，<mark style="color:orange;">而 StringBuilder 是非线程安全的</mark>，但 <mark style="color:orange;">StringBuilder 的性能却高于 StringBuffe</mark>r，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

## HashMap

## ConcurrentHashMap

## TreeMap

