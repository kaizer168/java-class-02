#题目01

##请你用自己的语言介绍Java运行时数据区(内存区域)

###堆(Heap):

创建对象的运行时数据区，由多个线程共享，含新生代，老年代，有垃圾回收机制。

虚拟机栈:

线程独享，每一个线程都有一个虚拟机栈，与线程一起创建，以栈帧的数据结构用来存储局部变量表丶操作栈丶动态连接丶返回地址。

本地方法栈:

线程独享，存储Native(C++)本地方法代码。

方法区(永久代，元空间):

线程共享，含类相关信息丶运行时常量池和编译后的代码缓存。JDK1.8 元空间取代了永久代。

运行时常量池(字符串常量池):

全局只有一个的StringTable，在堆里面，可以重复使用的常量字符串。

直接內存:

不在JVM堆里面的內存，直接读写内存，分配慢，读写快。

为什么堆内存要分年轻代和老年代?

那是基于以下的假说:

弱分代假说：绝大多数对象生命都很短暂(新生代可以频繁回收垃圾)

强分代假说：熬过多次垃圾回收依然存活的对象就越难以消亡(老年代不必频繁回收垃圾)。


题目02

描述一个Java对象的生命周期

对象的创建，內存分配，销毁。

解释一个对象的创建过程

New一个对象，检查常量池有没有这个类的引用，如果有，是否已加载，如果末加载或不在常量池就加载类，如果已加载就分配内存空间。

解释一个对象的內存分配

如果GC不带压缩功能就用指针碰撞来分配连续的內存空间。
如果GC带压缩功能就用空闲列表来分配不连续的內存空间。
把內存空间初始化为零值，再设置必要信息。

解释一个对象的销毁过程

沒有被引用的对象会被垃圾收集器回收。

对象的2种访问方式是什么?

1。句柄

2。直接指针

为什么需要內存担保?

如果新生代內存不夠分配，可以把新生代对象转移到老年代，以腾出新生代空间给新对象，或者直接进入老年代。

题目03

垃圾收集算法有哪些?

1。标记清除算法(Mark-Sweep)：先把垃圾对象标记出来，再清除已标记的对象

2。复制算法(Copying)：把內存分成两半，清除时把非垃圾对象复制到另一半

3。标记整理算法(Mark-Compact)：标记清除之后，再把活下来的对象整理整齐，减少碎片化

垃圾收集器有那些?他们的特点是什么?

1。Serial新生代收集器，单线程执行，使用复制算法，收集时必须暂停用户线程

2。ParNew收集器，新生代用并行复制算法，老年代用串行标记整理算法，适合多核CPU

3。ParallelScavenge新生代使用并行收集器，使用复制算法，吞吐量优先，收集时必须暂停用户线程。老年代使用串行收集器，使用标记整理算法

4。SerialOld老年代收集器，单线程执行，使用标记整理算法，收集时必须暂停用户线程

5。ParallelOld收集器，吞吐量优先，收集时必须暂停用户线程。新生代使用并行收集器，使用复制算法，老年代使用并行收集器，使用标记整理算法

6。CMS收集器，响应优先，不关心新生代，老年代采用标记清除算法

7。G1收集器，适合企业大內存(堆內存2GB-64GB)的服务端应用，兼顾吞吐量和低延时，全局使用标记整理算法，局部使用复制算法。使用者可以指定GC消耗时间。Region区类型可以分为Eden，Survivor，Old，Humongous，对Region的回收价值和成本排序，以达到用户指定的GC时间。

8。ZGC，可扩展的低延迟收集器，最大暂停时间小于1ms，並且不会因为內存增加而时间增加，适合內存8MB-16TB，基于Region结构，使用标记整理算法。发佈于JDK15。
