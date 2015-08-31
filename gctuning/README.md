# HotSpot VM GC调优指南-翻译

英文版来自于官方文档, 详情请参考下方的 [相关链接](#referlink)

## <a name="smallpagetitle">目录</a>

<div>

	<h3><a href="title.html">Title and Copyright Information</a></h3>
	<h3><a href="preface.html#gct_preface">Preface</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="preface.html#gct_audience">Audience</a>
		</li>
		<li>
			<a href="preface.html#gct_accessibility">Documentation Accessibility</a>
		</li>
		<li>
			<a href="preface.html#gct_related">Related Documents</a>
		</li>
		<li>
			<a href="preface.html#gct_conventions">Conventions</a>
		</li>
	</ul>
	<h3><a href="introduction.html#sthref3"><span>1</span> Introduction</a></h3>
	<h3><a href="ergonomics.html#ergonomics"><span>2</span> Ergonomics</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="ergonomics.html#sthref5">Garbage Collector, Heap, and Runtime Compiler Default Selections</a>
		</li>
		<li>
			<a href="ergonomics.html#sthref11">Behavior-Based Tuning</a>
			<ul style="list-style-type:none">
				<li>
					<a href="ergonomics.html#sthref12">Maximum Pause Time Goal</a>
				</li>
				<li>
					<a href="ergonomics.html#sthref13">Throughput Goal</a>
				</li>
				<li>
					<a href="ergonomics.html#sthref14">Footprint Goal</a>
				</li>
			</ul>
		</li>
		<li>
			<a href="ergonomics.html#sthref15">Tuning Strategy</a>
		</li>
	</ul>
	<h3><a href="generations.html#sthref16"><span>3</span> Generations</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="generations.html#sthref19">Performance Considerations</a>
		</li>
		<li>
			<a href="generations.html#sthref20">Measurement</a>
		</li>
	</ul>
	<h3><a href="sizing.html#sizing_generations"><span>4</span> Sizing the Generations</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="sizing.html#sthref22">Total Heap</a>
		</li>
		<li>
			<a href="sizing.html#sthref24">The Young Generation</a>
			<ul style="list-style-type:none">
				<li>
					<a href="sizing.html#sthref25">Survivor Space Sizing</a>
				</li>
			</ul>
		</li>
	</ul>
	<h3><a href="collectors.html#sthref27"><span>5</span> Available Collectors</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="collectors.html#sthref28">Selecting a Collector</a>
		</li>
	</ul>
	<h3><a href="parallel.html#parallel_collector"><span>6</span> The Parallel Collector</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="parallel.html#parallel_collector_generations">Generations</a>
		</li>
		<li>
			<a href="parallel.html#parallel_collector_ergonomics">Parallel Collector Ergonomics</a>
			<ul style="list-style-type:none">
				<li>
					<a href="parallel.html#parallel_collector_priority">Priority of Goals</a>
				</li>
				<li>
					<a href="parallel.html#parallel_collector_gen_size">Generation Size Adjustments</a>
				</li>
				<li>
					<a href="parallel.html#default_heap_size">Default Heap Size</a>
					<ul style="list-style-type:none">
						<li>
							<a href="parallel.html#sthref30">Client JVM Default Initial and Maximum Heap Sizes</a>
						</li>
						<li>
							<a href="parallel.html#sthref31">Server JVM Default Initial and Maximum Heap Sizes</a>
						</li>
						<li>
							<a href="parallel.html#sthref32">Specifying Initial and Maximum Heap Sizes</a>
						</li>
					</ul>
				</li>
			</ul>
		</li>
		<li>
			<a href="parallel.html#parallel_collector_excessive_gc">Excessive GC Time and OutOfMemoryError</a>
		</li>
		<li>
			<a href="parallel.html#parallel_collector_measurements">Measurements</a>
		</li>
	</ul>
	<h3><a href="concurrent.html#mostly_concurrent"><span>7</span> The Mostly Concurrent Collectors</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="concurrent.html#sthref33">Overhead of Concurrency</a>
		</li>
		<li>
			<a href="concurrent.html#sthref34">Additional References</a>
		</li>
	</ul>
	<h3><a href="cms.html#concurrent_mark_sweep_cms_collector"><span>8</span> Concurrent Mark Sweep (CMS) Collector</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="cms.html#concurrent_mode_failure">Concurrent Mode Failure</a>
		</li>
		<li>
			<a href="cms.html#sthref35">Excessive GC Time and OutOfMemoryError</a>
		</li>
		<li>
			<a href="cms.html#sthref36">Floating Garbage</a>
		</li>
		<li>
			<a href="cms.html#sthref37">Pauses</a>
		</li>
		<li>
			<a href="cms.html#sthref38">Concurrent Phases</a>
		</li>
		<li>
			<a href="cms.html#sthref39">Starting a Concurrent Collection Cycle</a>
		</li>
		<li>
			<a href="cms.html#sthref40">Scheduling Pauses</a>
		</li>
		<li>
			<a href="cms.html#CJAGIIEJ">Incremental Mode</a>
			<ul style="list-style-type:none">
				<li>
					<a href="cms.html#sthref41">Command-Line Options</a>
				</li>
				<li>
					<a href="cms.html#i_cms_recommended_options">Recommended Options</a>
				</li>
				<li>
					<a href="cms.html#sthref43">Basic Troubleshooting</a>
				</li>
			</ul>
		</li>
		<li>
			<a href="cms.html#sthref45">Measurements</a>
		</li>
	</ul>
	<h3><a href="g1_gc.html#garbage_first_garbage_collection"><span>9</span> Garbage-First Garbage Collector</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="g1_gc.html#allocation_evacuation_failure">Allocation (Evacuation) Failure</a>
		</li>
		<li>
			<a href="g1_gc.html#sthref47">Floating Garbage</a>
		</li>
		<li>
			<a href="g1_gc.html#pauses">Pauses</a>
		</li>
		<li>
			<a href="g1_gc.html#sthref48">Card Tables and Concurrent Phases</a>
		</li>
		<li>
			<a href="g1_gc.html#sthref49">Starting a Concurrent Collection Cycle</a>
		</li>
		<li>
			<a href="g1_gc.html#pause_time_goal">Pause Time Goal</a>
		</li>
	</ul>
	<h3><a href="g1_gc_tuning.html#g1_gc_tuning"><span>10</span> Garbage-First Garbage Collector Tuning</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="g1_gc_tuning.html#sthref50">Garbage Collection Phases</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#sthref51">Young Garbage Collections</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#sthref52">Mixed Garbage Collections</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#sthref53">Phases of the Marking Cycle</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#important_defaults">Important Defaults</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#how_to_unlock_experimental_vm_flags">How to Unlock Experimental VM Flags</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#recommendations">Recommendations</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#sthref61">Overflow and Exhausted Log Messages</a>
		</li>
		<li>
			<a href="g1_gc_tuning.html#humongous">Humongous Objects and Humongous Allocations</a>
		</li>
	</ul>
	<h3><a href="considerations.html#sthref62"><span>11</span> Other Considerations</a></h3>
	<ul style="list-style-type:none">
		<li>
			<a href="considerations.html#sthref63">Finalization and Weak, Soft, and Phantom References</a>
		</li>
		<li>
			<a href="considerations.html#sthref64">Explicit Garbage Collection</a>
		</li>
		<li>
			<a href="considerations.html#sthref65">Soft References</a>
		</li>
		<li>
			<a href="considerations.html#sthref66">Class Metadata</a>
		</li>
	</ul>
	<hr>

</div>



### <a name="referlink">相关链接</a>


调优指南: [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/)


翻译地址: [https://github.com/cncounter/GC-Tuning-Guide-CN](https://github.com/cncounter/GC-Tuning-Guide-CN)

翻译小组: [https://github.com/cncounter](https://github.com/cncounter)

翻译日期: [ 2015-09-01 ] ~ [ 预计  2016-01-01 ]

