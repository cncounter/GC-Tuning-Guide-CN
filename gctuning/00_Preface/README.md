# 前言

英文版来自于官方文档, 详情请参考下方的 [相关链接](#referlink)


<div>
	<a id="CHDBHEGA" name="CHDBHEGA"> </a><a id="gct_preface" name="gct_preface"></a>
	<p>
		The <i>Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide</i> describes the garbage collection methods included in the Java HotSpot Virtual Machine (Java HotSpot VM) and helps you determine which one is the best for your needs.
	</p>
	<div>
		<h2>Audience</h2>
		<p>
			This document is intended for application developers and system administrators who want to improve the performance of applications, especially those that handle large amounts of data, use many threads, and have high transaction rates.
		</p>
	</div><a id="gct_audience" name="gct_audience"> </a></a>
	<div>
		<a id="gct_accessibility" name="gct_accessibility"> <h2>Documentation Accessibility</h2> </a>
		<p>
			For information about Oracle's commitment to accessibility, visit the Oracle Accessibility Program website at <code dir="ltr">
			</code><code dir="ltr"><a href="http://www.oracle.com/pls/topic/lookup?ctx=acc&id=docacc">http://www.oracle.com/pls/topic/lookup?ctx=acc&amp;id=docacc</a></code>
			.
		</p>
		<a id="sthref2" name="sthref2"> </a>
		<p>
			<b>Access to Oracle Support</b>
		</p>
		<p>
			Oracle customers that have purchased support have access to electronic support through My Oracle Support. For information, visit <code dir="ltr">
			</code><code dir="ltr"><a href="http://www.oracle.com/pls/topic/lookup?ctx=acc&id=info">http://www.oracle.com/pls/topic/lookup?ctx=acc&amp;id=info</a></code>
			or visit <code dir="ltr"><a href="http://www.oracle.com/pls/topic/lookup?ctx=acc&id=trs">http://www.oracle.com/pls/topic/lookup?ctx=acc&amp;id=trs</a></code>
			if you are hearing impaired.
		</p>
	</div>

	<div>
		<a id="gct_related" name="gct_related">  </a><h2>Related Documents</h2>
		<p>
			For more information, see the following documents:
		</p>
		<ul>
			<li>
				
				<p>
					Java Virtual Machine Technology
					<br>
					<code dir="ltr">
					</code><code dir="ltr"><a href="http://docs.oracle.com/javase/8/docs/technotes/guides/vm/index.html">http://docs.oracle.com/javase/8/docs/technotes/guides/vm/index.html</a></code>
				</p>
			</li>
			<li>
				<p>
					Java SE HotSpot at a Glance
					<br>
					<code dir="ltr"><a href="http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html">http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html</a></code>
				</p>
			</li>
			<li>
				<p>
					HotSpot VM Frequently Asked Questions (FAQ)
				</p>
				<p>
					<code dir="ltr"><a href="http://www.oracle.com/technetwork/java/hotspotfaq-138619.html">http://www.oracle.com/technetwork/java/hotspotfaq-138619.html</a></code>
				</p>
			</li>
			<li>
				<p>
					How to Handle Java Finalization's Memory-Retention Issues: Covers finalization pitfalls and ways to avoid them.
				</p>
				<p>
					<code dir="ltr"><a href="http://www.devx.com/Java/Article/30192">http://www.devx.com/Java/Article/30192</a></code>
				</p>
			</li>
			<li>
				<p>
					Richard Jones and Rafael Lins, <i>Garbage Collection: Algorithms for Automated Dynamic Memory Management</i>, Wiley and Sons (1996), ISBN 0-471-94148-4
				</p>
			</li>
		</ul>
	</div>

	<div>
		<a id="gct_conventions" name="gct_conventions"> </a> <h2>Conventions</h2>
		<p>
			The following text conventions are used in this document:
		</p>
		<div>
			<table border="1" cellpadding="3" cellspacing="0" dir="ltr" frame="hsides" rules="groups" summary="This table summarizes conventions used in this document." title="Conventions Table" width="100%">
				<colgroup>
					<col width="24%">
					<col width="*">
				</colgroup>
				<thead>
					<tr align="left" valign="top">
						<th align="left" id="r1c1-t2" valign="bottom">Convention</th>
						<th align="left" id="r1c2-t2" valign="bottom">Meaning</th>
					</tr>
				</thead>
				<tbody>
					<tr align="left" valign="top">
						<td align="left" headers="r1c1-t2" id="r2c1-t2"><b>boldface</b></td>
						<td align="left" headers="r2c1-t2 r1c2-t2">Boldface type indicates graphical user interface elements associated with an action, or terms defined in text or the glossary.</td>
					</tr>
					<tr align="left" valign="top">
						<td align="left" headers="r1c1-t2" id="r3c1-t2"><i>italic</i></td>
						<td align="left" headers="r3c1-t2 r1c2-t2">Italic type indicates book titles, emphasis, or placeholder variables for which you supply particular values.</td>
					</tr>
					<tr align="left" valign="top">
						<td align="left" headers="r1c1-t2" id="r4c1-t2">
						<code dir="ltr">
							monospace
						</code></td>
						<td align="left" headers="r4c1-t2 r1c2-t2">Monospace type indicates commands within a paragraph, URLs, code in examples, text that appears on the screen, or text that you enter.</td>
					</tr>
				</tbody>
			</table>
			<br>
		</div>
	</div>
</div>





### <a name="referlink">相关链接</a>


调优指南: [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](http://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/)


翻译地址: [https://github.com/cncounter/GC-Tuning-Guide-CN](https://github.com/cncounter/GC-Tuning-Guide-CN)

翻译小组: [https://github.com/cncounter](https://github.com/cncounter)

翻译日期: [ 2015-09-01 ] ~ [ 预计  2016-01-01 ]

