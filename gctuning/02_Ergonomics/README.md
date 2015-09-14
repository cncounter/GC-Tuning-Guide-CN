
英文版来自于官方文档, 详情请参考下方的 [相关链接](#referlink)


# 2 工程学(Ergonomics)

工程学 是对Java虚拟机(JVM)和垃圾收集调优的过程, 比如基于行为的优化,提高应用程序的性能等等。JVM对垃圾收集器，堆内存大小与运行时编译器 根据平台的不同提供了不同的默认选择。这些默认选择根据不同类型的应用程序需要而减少需要使用的命令行参数。此外,基于行为的动态优化堆内存的大小,以适应不同应用程序的需求。


Ergonomics is the process by which the Java Virtual Machine (JVM) and garbage collection tuning, such as behavior-based tuning, improve application performance. The JVM provides platform-dependent default selections for the garbage collector, heap size, and runtime compiler. These selections match the needs of different types of applications while requiring less command-line tuning. In addition, behavior-based tuning dynamically tunes the sizes of the heap to meet a specified behavior of the application.


本节介绍这些默认选择和基于行为的调优。在随后章节介绍更详细的控制选项之前, 我们先使用这些默认值。


This section describes these default selections and behavior-based tuning. Use these defaults first before using the more detailed controls described in subsequent sections.

## 默认的垃圾收集器, 堆内存大小与运行时编译器

## Garbage Collector, Heap, and Runtime Compiler Default Selections

在JDK看来, 服务器级(server-class) 的机器具有如下配置:

A class of machine referred to as a server-class machine has been defined as a machine with the following:

- 2个及以上的物理处理器

- 2 or more physical processors

- 2GB 及以上的物理内存

- 2 or more GB of physical memory

在服务器上,默认选项包括: 

On server-class machines, the following are selected by default:

- 吞吐量高的垃圾收集器

- Throughput garbage collector

- 初始化堆内存为物理内存的 1/64, 最高为 1GB(默认值)

- Initial heap size of 1/64 of physical memory up to 1 GB

- 最大堆内存为物理内存的 1/64, 最多 1GB(默认值)

- Maximum heap size of 1/4 of physical memory up to 1 GB

- 服务器运行时编译器

- Server runtime compiler

关于64位系统的初始化堆内存和堆内存限制,请参考 [第6章 并行垃圾收集器 中的 默认堆内存 ](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#default_heap_size)

For initial heap and maximum heap sizes for 64-bit systems, see the section [Default Heap](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#default_heap_size) Size in [The Parallel Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#CHDCFBIF).

**服务器级**的默认设置，除32位 Windows 系统外都适用。[表 2-1, "默认运行时编译器"](#BABIIFHA) 显示了在各个系统版本上的默认值.

The definition of a server-class machine applies to all platforms with the exception of 32-bit platforms running a version of the Windows operating system. [Table 2-1, "Default Runtime Compiler"](#BABIIFHA), shows the choices made for the runtime compiler for different platforms.

> ### <a name="BABIIFHA">表 2-1 默认运行时编译器</a>



<table border="1" cellpadding="3" cellspacing="0" dir="ltr" frame="border" rules="all" summary="Default runtime compiler" title="Default Runtime Compiler" width="100%">
	<col width="*"/>
	<col width="25%"/>
	<col width="25%"/>
	<col width="25%"/>
	<thead>
		<tr align="left" valign="top">
			<th align="left" id="r1c1-t2" valign="bottom">Platform</th>
			<th align="left" id="r1c2-t2" valign="bottom">Operating System</th>
			<th align="left" id="r1c3-t2" valign="bottom">Default<a href="#BABFAAII" id="BABFAAII" name="BABFAAII" onclick="footdisplay(1,&quot;Client means the client runtime compiler is used. Server means the server runtime compiler is used. &quot;)"><sup>Foot1</sup></a></th>
			<th align="left" id="r1c4-t2" valign="bottom">Default if Server-Class<a href="#sthref7" id="sthref7" name="sthref7" onclick="footdisplay(1,&quot;Client means the client runtime compiler is used. Server means the server runtime compiler is used. &quot;)"><sup>Footref1</sup></a></th>
		</tr>
	</thead>
	<tbody>
		<tr align="left" valign="top">
			<td align="left" headers="r1c1-t2" id="r2c1-t2">
			<p>
				i586(即32-bit)
			</p></td>
			<td align="left" headers="r2c1-t2 r1c2-t2">
			<p>
				Linux
			</p></td>
			<td align="left" headers="r2c1-t2 r1c3-t2">
			<p>
				Client
			</p></td>
			<td align="left" headers="r2c1-t2 r1c4-t2">
			<p>
				Server
			</p></td>
		</tr>
		<tr align="left" valign="top">
			<td align="left" headers="r1c1-t2" id="r3c1-t2">
			<p>
				i586(即32-bit)
			</p></td>
			<td align="left" headers="r3c1-t2 r1c2-t2">
			<p>
				Windows
			</p></td>
			<td align="left" headers="r3c1-t2 r1c3-t2">
			<p>
				Client
			</p></td>
			<td align="left" headers="r3c1-t2 r1c4-t2">
			<p>
				Client<a href="#sthref8" id="sthref8" name="sthref8" onclick="footdisplay(2,&quot;The policy was chosen to use the client runtime compiler even on a server class machine. This choice was made because historically client applications (for example, interactive applications) were run more often on this combination of platform and operating system.&quot;)"><sup>Foot2</sup></a>
			</p></td>
		</tr>
		<tr align="left" valign="top">
			<td align="left" headers="r1c1-t2" id="r4c1-t2">
			<p>
				SPARC (64-bit)
			</p></td>
			<td align="left" headers="r4c1-t2 r1c2-t2">
			<p>
				Solaris
			</p></td>
			<td align="left" headers="r4c1-t2 r1c3-t2">
			<p>
				Server
			</p></td>
			<td align="left" headers="r4c1-t2 r1c4-t2">
			<p>
				Server<a href="#BABIFJCI" id="BABIFJCI" name="BABIFJCI" onclick="footdisplay(3,&quot;Only the server runtime compiler is supported.&quot;)"><sup>Foot3</sup></a>
			</p></td>
		</tr>
		<tr align="left" valign="top">
			<td align="left" headers="r1c1-t2" id="r5c1-t2">
			<p>
				AMD (64-bit)
			</p></td>
			<td align="left" headers="r5c1-t2 r1c2-t2">
			<p>
				Linux
			</p></td>
			<td align="left" headers="r5c1-t2 r1c3-t2">
			<p>
				Server
			</p></td>
			<td align="left" headers="r5c1-t2 r1c4-t2">
			<p>
				Server<a href="#sthref9" id="sthref9" name="sthref9" onclick="footdisplay(3,&quot;Only the server runtime compiler is supported.&quot;)"><sup>Footref3</sup></a>
			</p></td>
		</tr>
		<tr align="left" valign="top">
			<td align="left" headers="r1c1-t2" id="r6c1-t2">
			<p>
				AMD (64-bit)
			</p></td>
			<td align="left" headers="r6c1-t2 r1c2-t2">
			<p>
				Windows
			</p></td>
			<td align="left" headers="r6c1-t2 r1c3-t2">
			<p>
				Server
			</p></td>
			<td align="left" headers="r6c1-t2 r1c4-t2">
			<p>
				Server<a href="#sthref10" id="sthref10" name="sthref10" onclick="footdisplay(3,&quot;Only the server runtime compiler is supported.&quot;)"><sup>Footref3</sup></a>
			</p></td>
		</tr>
	</tbody>
</table>


>Footnote1: Client 模式的意思是使用 client runtime compiler. Server 模式的意思是使用 server runtime compiler.

>Footnote2: 即便在服务器级别的机器上, Win32系统的默认选择仍然是 client runtime compiler . 这是因为要兼容以前的应用系统(例如, 交互式应用程序), 而且在 win32 系统上的程序大多是这一类的.

>Footnote3: SPARC 平台只支持 server runtime compiler.


## 基于行为的调整( Behavior-Based Tuning)

对于并行收集器,Java SE提供了两个针对特定应用行为的调优参数:  **最大停顿时间指标** 和 吞**吐量指标**; 详情请参考 [The Parallel Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#CHDCFBIF) 章节。(这两个选项在其他垃圾收集器上不可用.) 注意, 这些行为并不能一定会得到满足。原因在于程序需要一个足够大的堆来存放所有实时数据。此外, 最小堆内存参数的设置也可能会阻止达成这些期望的目标。


For the parallel collector, Java SE provides two garbage collection tuning parameters that are based on achieving a specified behavior of the application: maximum pause time goal and application throughput goal; see the section [The Parallel Collector](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/parallel.html#CHDCFBIF). (These two options are not available in the other collectors.) Note that these behaviors cannot always be met. The application requires a heap large enough to at least hold all of the live data. In addition, a minimum heap size may preclude reaching these desired goals.

### 最大停顿时间指标(Maximum Pause Time Goal)

The pause time is the duration during which the garbage collector stops the application and recovers space that is no longer in use. The intent of the maximum pause time goal is to limit the longest of these pauses. An average time for pauses and a variance on that average is maintained by the garbage collector. The average is taken from the start of the execution but is weighted so that more recent pauses count more heavily. If the average plus the variance of the pause times is greater than the maximum pause time goal, then the garbage collector considers that the goal is not being met.

The maximum pause time goal is specified with the command-line option -XX:MaxGCPauseMillis=<nnn>. This is interpreted as a hint to the garbage collector that pause times of <nnn> milliseconds or less are desired. The garbage collector will adjust the Java heap size and other parameters related to garbage collection in an attempt to keep garbage collection pauses shorter than <nnn> milliseconds. By default there is no maximum pause time goal. These adjustments may cause garbage collector to occur more frequently, reducing the overall throughput of the application. The garbage collector tries to meet any pause time goal before the throughput goal. In some cases, though, the desired pause time goal cannot be met.

### 吞吐量指标(Throughput Goal)

The throughput goal is measured in terms of the time spent collecting garbage and the time spent outside of garbage collection (referred to as application time). The goal is specified by the command-line option -XX:GCTimeRatio=<nnn>. The ratio of garbage collection time to application time is 1 / (1 + <nnn>). For example, -XX:GCTimeRatio=19 sets a goal of 1/20th or 5% of the total time for garbage collection.

The time spent in garbage collection is the total time for both the young generation and old generation collections combined. If the throughput goal is not being met, then the sizes of the generations are increased in an effort to increase the time that the application can run between collections.

### 记录指标(Footprint Goal)

如果满足了吞吐量指标和最大停顿时间指标，那么垃圾收集器会减小堆内存，直到其中的一个指标不能满足为止(其实只会是吞吐量指标)。不能满足的这个临界点将会被记录下来。

If the throughput and maximum pause time goals have been met, then the garbage collector reduces the size of the heap until one of the goals (invariably the throughput goal) cannot be met. The goal that is not being met is then addressed.

## Tuning Strategy
Do not choose a maximum value for the heap unless you know that you need a heap greater than the default maximum heap size. Choose a throughput goal that is sufficient for your application.

The heap will grow or shrink to a size that will support the chosen throughput goal. A change in the application's behavior can cause the heap to grow or shrink. For example, if the application starts allocating at a higher rate, the heap will grow to maintain the same throughput.

If the heap grows to its maximum size and the throughput goal is not being met, the maximum heap size is too small for the throughput goal. Set the maximum heap size to a value that is close to the total physical memory on the platform but which does not cause swapping of the application. Execute the application again. If the throughput goal is still not met, then the goal for the application time is too high for the available memory on the platform.

If the throughput goal can be met, but there are pauses that are too long, then select a maximum pause time goal. Choosing a maximum pause time goal may mean that your throughput goal will not be met, so choose values that are an acceptable compromise for the application.

It is typical that the size of the heap will oscillate as the garbage collector tries to satisfy competing goals. This is true even if the application has reached a steady state. The pressure to achieve a throughput goal (which may require a larger heap) competes with the goals for a maximum pause time and a minimum footprint (which both may require a small heap).












### <a name="referlink">相关链接</a>


调优指南: [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/)


翻译地址: [https://github.com/cncounter/GC-Tuning-Guide-CN](https://github.com/cncounter/GC-Tuning-Guide-CN)

翻译小组: [https://github.com/cncounter](https://github.com/cncounter)

翻译日期: [ 2015-09-01 ] ~ [ 预计  2016-01-01 ]

