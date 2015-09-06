
英文版来自于官方文档, 详情请参考下方的 [相关链接](#referlink)

# 1 简介

Java 平台应用得非常广泛, 例如个人电脑上运行的小小的 applet, 或者是大型服务器上部署的 WEB 服务。

A wide variety of applications use Java Platform, Standard Edition (Java SE), from small applets on desktops to web services on large servers. 

为了支持各种不同类型的部署环境, Java 虚拟机(Java HotSpot VM) 提供了很多种类的垃圾收集器。每一种都是为某种特定需求而设计的。

In support of this diverse range of deployments, the Java HotSpot virtual machine implementation (Java HotSpot VM) provides multiple garbage collectors, each designed to satisfy different requirements. 

这也是适应各种规模应用程序而实现的一个重要部分。

This is an important part of meeting the demands of both large and small applications. 

Java SE 会根据运行平台而选择默认最优的垃圾收集器。

Java SE selects the most appropriate garbage collector based on the class of the computer on which the application is run. 

但是这种普适性选择并不是对每个程序都是最优解。

However, this selection may not be optimal for every application. 

对于性能要求极高或者极端苛刻的用户，开发人员，记忆系统管理员就需要明确地指定使用哪款垃圾收集器，并对配置参数进行合理的优化。

Users, developers, and administrators with strict performance goals or other requirements may need to explicitly select the garbage collector and tune certain parameters to achieve the desired level of performance.

本文的主要目的是为这些调优任务提供帮助信息。

This document provides information to help with these tasks. 

首先， 在串行垃圾收集器章节中会介绍垃圾收集器的一般特性，以及基本的调优选项。

First, general features of a garbage collector and basic tuning options are described in the context of the serial, stop-the-world collector. 

然后会介绍选择垃圾收集器时应该考虑哪些因素。

Then specific features of the other collectors are presented along with factors to consider when selecting a collector.

垃圾收集器(GC, garbage collector)就是一种内存管理工具。

通过以下这些步骤来实现自动内存管理。

A garbage collector (GC) is a memory management tool. It achieves automatic memory management through the following operations:

- 在年轻代分配对象，并将存活一定时间的对象迁移到老年代之中去。

- Allocating objects to a young generation and promoting aged objects into an old generation.

- 在老年代中在并发/并行标记阶段找出所有存活的对象。 HotSpot 虚拟机在内存占用达到一定比例时就会触发标记阶段的执行。详情请参考 [Concurrent Mark Sweep (CMS) Collector](../08_CMS_Collector/) 和 [Garbage-First Garbage Collector](../09_G1_Garbage_Collector/) 章节.

- Finding live objects in the old generation through a concurrent (parallel) marking phase. The Java HotSpot VM triggers the marking phase when the total Java heap occupancy exceeds the default threshold. See the sections [Concurrent Mark Sweep (CMS) Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html#concurrent_mark_sweep_cms_collector) and [Garbage-First Garbage Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/g1_gc.html#garbage_first_garbage_collection).

- 通过并行拷贝来压缩对象存储空间并释放内存, 详情请参考 [Parallel Collector](../06_The_Parallel_Collector/) 和 [Garbage-First Garbage Collector](../09_G1_Garbage_Collector/) 章节.

- Recovering free memory by compacting live objects through parallel copying. See the sections The [Parallel Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#CHDCFBIF) and [Garbage-First Garbage Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/g1_gc.html#garbage_first_garbage_collection)


选择一款合适的垃圾收集器有多重要?

When does the choice of a garbage collector matter?

对于某些程序来说，什么用也没有。

For some applications, the answer is never. 

也就是说，垃圾收集所占用的时间和产生的停顿对他们来说基本上没什么影响。

That is, the application can perform well in the presence of garbage collection with pauses of modest frequency and duration. 

但对于复杂一点的程序来说就不一样了， 比如说大数据处理，多线程应用，或者是及时响应的事务处理程序。

However, this is not the case for a large class of applications, particularly those with large amounts of data (multiple gigabytes), many threads, and high transaction rates.


Amdahl 定律(并行所能提升的速度在具体情况下受串行执行部分的限制) 指明了大部分情况下并不能完全进行完全的并行化, 总有一部分代码是串行执行的,不能从并行化中受益。 这同样适用于Java平台。特别是,Java 1.4 之前的虚拟机不支持并行垃圾回收, 所以在多处理器系统上的垃圾收集也和另一个并行程序关联密切。


Amdahl's law (parallel speedup in a given problem is limited by the sequential portion of the problem) implies that most workloads cannot be perfectly parallelized; some portion is always sequential and does not benefit from parallelism. This is also true for the Java platform. In particular, virtual machines from Oracle for the Java platform prior to Java SE 1.4 do not support parallel garbage collection, so the effect of garbage collection on a multiprocessor system grows relative to an otherwise parallel application.

图 1-1 是一种理想化系统模型。除 GC 之外的代码部分都能完美地进行并行化。 最上面的红线指的是在单核系统上,垃圾回收只占用了 1% 的运行时间。 但是在CPU扩展到32核以后,就会有超过 20% 的性能损失。 中间品红色的线表示只要有 10% 的时间花在垃圾收集上面，那么扩展到32核以后，系统的吞吐量就会下降75%以上(还不去考虑在单核处理器程序上所花费的垃圾收集时间)。

The graph in [Figure 1-1, "Comparing Percentage of Time Spent in Garbage Collection"](#Figure1-1) models an ideal system that is perfectly scalable with the exception of garbage collection (GC). The red line is an application spending only 1% of the time in garbage collection on a uniprocessor system. This translates to more than a 20% loss in throughput on systems with 32 processors. The magenta line shows that for an application at 10% of the time in garbage collection (not considered an outrageous amount of time in garbage collection in uniprocessor applications), more than 75% of throughput is lost when scaling up to 32 processors.


**<a name="Figure1-1"> </a>图 1-1 垃圾回收的时间占比在处理器数量增大后对吞吐量的影响**

**<a name="Figure1-1"> </a>Figure 1-1 Comparing Percentage of Time Spent in Garbage Collection**

![](./gc_spent.png)


关于 [图 1-1 的详细说明请点击这里](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/img_text/jsgct_dt_005_gph_pc_vs_tp.html)


[Description of "Figure 1-1 Comparing Percentage of Time Spent in Garbage Collection"](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/img_text/jsgct_dt_005_gph_pc_vs_tp.html)


这个图说明的是,在小型系统上开发的程序，也许只是微不足道的速度问题，当扩大到大型系统时可能会成为主要的性能瓶颈。

This shows that negligible speed issues when developing on small systems may become principal bottlenecks when scaling up to large systems. 

但也许只需要小小的改进就能减少这样的瓶颈，在性能上获得巨大的收益。在足够大的系统上, 选择合适的垃圾收集器,并做必要的调优将会变得非常必要。

However, small improvements in reducing such a bottleneck can produce large gains in performance. For a sufficiently large system, it becomes worthwhile to select the right garbage collector and to tune it if necessary.


串行收集器通常来说足以应付那些 “小” 程序(只需要不到100MB内存空间)。

The serial collector is usually adequate for most "small" applications (those requiring heaps of up to approximately 100 megabytes (MB (on modern processors). 

其他的垃圾收集器都会有额外的开销或复杂性, 这就是专业行为的代价。

The other collectors have additional overhead or complexity, which is the price for specialized behavior.

如果应用程序不需要这些垃圾收集器的专业行为,建议使用串行收集器。

If the application does not need the specialized behavior of an alternate collector, use the serial collector. 

串行收集器不太适合用来处理运行在多核CPU上的大型应用程序, 特别是有大量线程，占用很多内存的那些应用。

One situation where the serial collector is not expected to be the best choice is a large, heavily threaded application that runs on a machine with a large amount of memory and two or more processors. 

当程序在服务器级别的机器上执行时, 默认会使用并行垃圾收集器。

When applications are run on such server-class machines, the parallel collector is selected by default.

详情请参考  [Ergonomics](../02_Ergonomics/) 章节.

See the section [Ergonomics](../02_Ergonomics/).

本文档最初是基于Java SE 8编写的，系统平台是 SPARC - Solaris。

This document was developed using Java SE 8 on the Solaris operating system (SPARC Platform Edition) as the reference. 

但相关的概念和建议适用于所有平台, 包括 Linux， Windows，X64版本的Solaris，以及 OS X。

However, the concepts and recommendations presented here apply to all supported platforms, including Linux, Microsoft Windows, the Solaris operating system (x64 Platform Edition), and OS X.

另外， 所使用的命令行参数/选型也适用于所有平台，当然，某些平台上这些参数的默认值会不一样。

In addition, the command line options mentioned are available on all supported platforms, although the default values of some options may be different on each platform.



### <a name="referlink">相关链接</a>


调优指南: [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/)


翻译地址: [https://github.com/cncounter/GC-Tuning-Guide-CN](https://github.com/cncounter/GC-Tuning-Guide-CN)

翻译小组: [https://github.com/cncounter](https://github.com/cncounter)

翻译日期: [ 2015-09-01 ] ~ [ 预计  2016-01-01 ]

