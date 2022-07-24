# Java 基础

## JDK和JRE有什么区别

* JDK：Java Development Kit 的简称，java 开发工具包，提供了 java 的开发环境和运行环境。
* JRE：Java Runtime Environment 的简称，java 运行环境，为 java 的运行提供了所需环境。

具体来说 JDK 其实包含了 JRE，同时还包含了编译 java 源码的编译器 javac，还包含了很多 java 程序调试和分析的工具。简单来说：如果你需要运行 java 程序，只需安装 JRE 就可以了，如果你需要编写 java 程序，需要安装 JDK。

## **== 和 equals 的区别是什么**

**== 解读**

对于基本类型和引用类型 == 的作用效果是不同的，如下所示：

* 基本类型：比较的是值是否相同；
* 引用类型：比较的是引用是否相同；

```java
String x = "string";
String y = "string";
String z = new String("string");
System.out.println(x==y); // true
System.out.println(x==z); // false
System.out.println(x.equals(y)); // true
System.out.println(x.equals(z)); // true
```

代码解读：因为 x 和 y 指向的是同一个引用，所以 == 也是 true，而 new String()方法则重写开辟了内存空间，所以 == 结果为 false，而 equals 比较的一直是值，所以结果都为 true。

**equals 解读**

equals 本质上就是 ==，只不过 String 和 Integer 等重写了 equals 方法，把它变成了值比较。看下面的代码就明白了。

首先来看默认情况下 equals 比较一个有相同值的对象，代码如下：

```java
class Cat {    
    public Cat(String name) {        
        this.name = name;    
    }    
    private String name;    
    public String getName() {
        return name;    
    }    
    public void setName(String name) {
        this.name = name;    
    }
}
Cat c1 = new Cat("王磊");
Cat c2 = new Cat("王磊");
System.out.println(c1.equals(c2)); // false
```

输出结果出乎我们的意料，竟然是 false？这是怎么回事，看了 equals 源码就知道了，源码如下：

```java
public boolean equals(Object obj) {    
    return (this == obj);
}
```

原来 equals 本质上就是 ==。

那问题来了，两个相同值的 String 对象，为什么返回的是 true？代码如下：

```java
String s1 = new String("老王");
String s2 = new String("老王");
System.out.println(s1.equals(s2)); // true
```

同样的，当我们进入 String 的 equals 方法，找到了答案，代码如下：

```java
public boolean equals(Object anObject) {    
    if (this == anObject) {        
        return true;    
    }    
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;        
        if (n == anotherString.value.length) {            
            char v1[] = value;            
            char v2[] = anotherString.value;            
            int i = 0;            
            while (n-- != 0) {                
                if (v1[i] != v2[i])                    
                return false;                
                i++;            
            }            
            return true;        
        }    
    }    
    return false;
    }
```

原来是 String 重写了 Object 的 equals 方法，把引用比较改成了值比较。

**总结** ：== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 equals 默认情况下是引用比较，只是很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等。

## **两个对象的 hashCode()相同，则 equals()也一定为 true，对吗？**

不对，两个对象的 hashCode()相同，equals()不一定 true。

代码示例：

```
String str1 = "通话";
String str2 = "重地";
System.out.println(String.format("str1：%d | str2：%d",  str1.hashCode(),str2.hashCode()));
System.out.println(str1.equals(str2));
```

执行的结果：

str1：1179395 | str2：1179395

false

代码解读：很显然“通话”和“重地”的 hashCode() 相同，然而 equals() 则为 false，因为在散列表中，hashCode()相等即两个键值对的哈希值相等，然而哈希值相等，并不一定能得出键值对相等。

## **final 在 java 中有什么作用？**

* final 修饰的类叫最终类，该类不能被继承。
* final 修饰的方法不能被重写。
* final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。

## **String 属于基础的数据类型吗？**

String 不属于基础类型，基础类型有 8 种：byte、boolean、char、short、int、float、long、double，而 String 属于对象**。**

## **java 中操作字符串都有哪些类？它们之间有什么区别？**

操作字符串的类有：String、StringBuffer、StringBuilder。

String 和 StringBuffer、StringBuilder 的区别在于 <mark style="color:orange;">String 声明的是不可变的对象</mark>，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 <mark style="color:orange;">StringBuffer、StringBuilder 可以在原有对象的基础上进行操作</mark>，所以在经常改变字符串内容的情况下最好不要使用 String。

StringBuffer 和 StringBuilder 最大的区别在于，<mark style="color:orange;">StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的</mark>，但 <mark style="color:orange;">StringBuilder 的性能却高于 StringBuffer</mark>，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

## **String str="i"与 String str=new String("i")一样吗？**

不一样，因为内存的分配方式不一样。String str="i"的方式，java 虚拟机会将其分配到常量池中；而 String str=new String("i") 则会被分到堆内存中。

## **java 中 IO 流分为几种？**

按功能来分：输入流（input）、输出流（output）。

按类型来分：字节流和字符流。

字节流和字符流的区别是：字节流按 8 位传输以字节为单位输入输出数据，字符流按 16 位传输以字符为单位输入输出数据。

## **BIO、NIO、AIO 有什么区别？**

* BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
* NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
* AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。
