# 判断对象是否为垃圾的算法

- 引用计数算法

  ![image-20200223043627545](assets/image-20200223043627545.png)

  - 优点：执行效率高，程序执行受影响小
  - 缺点：无法检测出循环引用的情况，导致内存泄露

- 可达性分析算法

![image-20200223044222618](assets/image-20200223044222618.png)

可以作为GC Root的对象

![image-20200223044316518](assets/image-20200223044316518.png)

 # 探探你了解的垃圾回收算法

- 标记-清除算法（Mark and Sweep）

  - 标记：从根集合进行扫描，对存活的对象进行标记
  - 清除：对堆内存从头到尾进行线性遍历，回收不可达对象内存
  - 缺点：容易造成碎片化

- 复制算法（Copying）

  - 分为对象面和空闲面
  - 对象在对象面上创建
  - 存活的对象被从对象面复制到空闲面
  - 将对象面所有对象内存清除 

  1. 解决碎片化的问题
  2. 顺序分配内存，简单高效
  3. 适用于对象存活率低的场景

- 标记-整理算法（Compacting）

  - 标记：从根集合进行扫描，对存活的对象进行标记
  - 清除：移动所有存活的对象，且按照内存地址次序依次排序，然后将末端内存地址以后的内存全部回收

  1. 避免内存的不连续行
  2. 不用设置两块内存互换
  3. 适用于存活率高的场景

- 分代收集算法（Generational Collector）

  - 垃圾回收算法的组合拳
  - 按照对象生命周期的不同划分区域以采用不同的垃圾回收算法
  - 目的：提高JVM的回收效率

![image-20200223054052865](assets/image-20200223054052865.png)

# GC的分类

- Minor GC
- Full GC

 # 年轻代

尽可能快速地收集掉那些生命周期短的对象

- Eden区
- 两个Survivor区

![image-20200223054410474](assets/image-20200223054410474.png)

## 对象如何晋升到老年代

- 经历一定Minor次数依然存活的对象
- Survivor区中存放不下的对象
- 新生成的大对象（`-XX:+PretenuerSizeThreshold`）

## 常用的调优参数

![image-20200223055144156](assets/image-20200223055144156.png)

 # 老年代

存放生命周期较长的对象

![image-20200223055548745](assets/image-20200223055548745.png)

## 触发Full GC的条件

![image-20200223062613569](assets/image-20200223062613569.png)

# 新生代垃圾收集器

## Stop-the-World

- JVM由于要执行GC而停止了应用程序的执行
- 任何一种GC算法中都会发生
- 多数GC优化通过减少Stop-the-world发生的时间来提高程序性能 

## Safepoint

- 分析过程中对象应引用关系不糊发生变化的点
- 产生Safepoint的地方：方法调用；循环跳转；异常跳转等
- 安全点数量得适中

## 常见的垃圾收集器

###  JVM的运行模式

- Server
- Client

## 垃圾收集器之间的联系

<img src="assets/image-20200223063855947.png" alt="image-20200223063855947" style="zoom:50%;" />

## Serial收集器（-XX+UseSerialGC，复制算法）

- 单线程收集，进行垃圾收集时，必须暂停所有工作线程
- 简单高效，Client模式下默认的年轻代收集器

![image-20200223064212241](assets/image-20200223064212241.png)

## ParNew收集器（-XX+UseParNewGC,复制算法）

![image-20200223064915064](assets/image-20200223064915064.png)

 ## Parallel Scavenge收集器（-XX：+UseParallelGC，复制算法）

- 吞吐量=运行用户代码时间/（运行用户代码时间+垃圾收集时间）

![image-20200223065358638](assets/image-20200223065358638.png)



# 老年代常见的垃圾收集器

## Serial Old 收集器（-XX：+UseSerialOldGC，标记-整理算法）

- 单线程收集，进行垃圾收集时，必须暂停所有工作线程
- 简单高效，Client模式下默认的老年代收集器

![image-20200223070028364](assets/image-20200223070028364.png)

## Parallel Old收集器（-XX：+UseParallelOldGC，标记-整理算法）

- 多线程，吞吐量优先

  ![image-20200223070606212](assets/image-20200223070606212.png)

## CMS收集器（-XX：+UseConcMarkSweepGC，标记-清除算法）

可能会出现内存空间碎片化的问题

![image-20200223095149468](assets/image-20200223095149468.png)

![image-20200223095201190](assets/image-20200223095201190.png)

## G1收集器（-XX：UseG1GC，复制+标记-整理算法）

G1收集器的特点

<img src="assets/image-20200223095336335.png" alt="image-20200223095336335" style="zoom:50%;" /> 

![image-20200223100111988](assets/image-20200223100111988.png)

# GC相关的面试题

# Object的finalize（）方法的作用是否与C++的析构函数作用相同

- 与C++的析构函数不同，析构函数调用确定，而它是不确定的
- 将未被引用的对象放置于F-Queue队列
- 方法执行随时可能会被终止
- 给予对象最后一次重生的机会 
- 不建议使用

# Java中的强引用，软引用，弱引用，虚引用有什么用

## 强引用

![image-20200223103751540](assets/image-20200223103751540.png)

## 软引用

![image-20200223103827413](assets/image-20200223103827413.png)

## 弱引用

![image-20200223103902443](assets/image-20200223103902443.png)

## 虚引用

![image-20200223105010415](assets/image-20200223105010415.png)

![image-20200223105521027](assets/image-20200223105521027.png)

![image-20200223105849547](assets/image-20200223105849547.png)

## 引用队列

![image-20200223113825816](assets/image-20200223113825816.png)