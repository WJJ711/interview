 

# 探探你对Java的理解

- 平台无关性
- GC
- 语言特性
- 面向对象
- 类库
- 异常处理

# 一次编译，到处运行如何实现 

![image-20200222133852729](assets/image-20200222133852729.png)

# JVM如何加载.class文件

![image-20200222140122801](assets/image-20200222140122801.png)

# 谈谈反射

![image-20200222141856116](assets/image-20200222141856116.png)

 # 类从编译到执行的过程

- 编译器将Robot.java源文件编译为Robot.class字节码文件
- ClassLoader将字节码转换为JVM中的Class<Robot>对象
- JVM利用Class<Robot>对象实例化为Robot对象

# 谈谈ClassLoader

类加载器

![image-20200222143806310](assets/image-20200222143806310.png)

 ## ClassLoder的种类

![image-20200222144836938](assets/image-20200222144836938.png)

![image-20200222145133377](assets/image-20200222145133377.png)

# 类的加载方式

- 隐式加载：new
- 显式加载：loadClass，forName等

# loadClass和forName的区别

类的装载过程

![image-20200222150643621](assets/image-20200222150643621.png)

- **Class.forName得到的class是已经初始化完成的**
- **Classloder.loadClass得到的class是还没有链接的** ,Spring IOC为了延迟加载，大量使用了loadClass，加快了加载速度

# 你了解Java内存模型么

## 内存简介

![image-20200223024244420](assets/image-20200223024244420.png)

## 地址空间的划分

- 内核空间
- 用户空间

![image-20200223024346164](assets/image-20200223024346164.png) 

 

![image-20200223024735134](assets/image-20200223024735134.png)

## 程序计数器

![image-20200223024838024](assets/image-20200223024838024.png)

## Java虚拟机栈（Stack）

![image-20200223024927647](assets/image-20200223024927647.png)

### 局部变量表和操作数栈

![image-20200223025041057](assets/image-20200223025041057.png)

## 元空间（MetaSpace）与永久代（PermGen）的区别

- **元空间使用本地内存，而永久代使用的是jvm内存**

![image-20200223031859566](assets/image-20200223031859566.png)

## 堆

![image-20200223032510616](assets/image-20200223032510616.png)

# JVM三大性能调优参数-Xms -Xmx -Xss的含义

- -Xss：规定了每个线程虚拟机栈（堆栈）的大小
- -Xms：堆的初始值
- -Xmx：堆能达到的最大值

# Java内存模型中堆和栈的区别——内存分配策略

![image-20200223033641226](assets/image-20200223033641226.png)

- **联系：引用对象、数组时，栈里定义变量保存堆中目标的首地址**

![image-20200223034006474](assets/image-20200223034006474.png)

### 区别

![image-20200223034144739](assets/image-20200223034144739.png)

**程序**

![image-20200223034721068](assets/image-20200223034721068.png)

![image-20200223034733884](assets/image-20200223034733884.png)

# 不同JDK版本之间intern（）方法的区别——JDK6 VS JDK6+

![image-20200223034953528](assets/image-20200223034953528.png)

## 程序代码

![image-20200223040347514](assets/image-20200223040347514.png)

JDK8 : false true

JDK6：false false

### JDK6时intern()

![image-20200223040507615](assets/image-20200223040507615.png)

### JDK7及以上

![image-20200223040802828](assets/image-20200223040802828.png)