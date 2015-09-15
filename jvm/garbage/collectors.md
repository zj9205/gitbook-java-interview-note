# 垃圾回收器
JVM采用的是**分代回收**，不同代有不同的垃圾收集器
![垃圾收集器](http://askingwindy-gitcafe.qiniudn.com/垃圾收集器.jpg)
##CMS(Concurrent Mark Sweep)
以获取**最短回收停顿**时间为目标的收集器
![CMS回收器](http://askingwindy-gitcafe.qiniudn.com/并发回收.jpg)
###基于标记-清除算法，分为四个步骤：
1. 初始标记
	-  Stop the world：用户不可见的情况下，把用户的正常进程全部停掉
	-  仅标记GC Roots能直接关联的对象
2. 并发标记
	- 进行GC Roots Tracing过程（时间特别长）
3. 重新标记
	- Stop the world
	- 修正并发标记期间变化的记录
4. 并发清除
	- 清除资源（时间特别长）

###缺点
* CPU资源敏感
* 无法处理浮动垃圾（伴随着程序运行产生的垃圾，在下次GC时清除）
* 处理完后有大量空间碎片

##G1
###G1原理与CMS类似
在G1的基础上：
1. 进行空间整合
	- 将JVM堆划分为多个大小相等的独立区域(Region)
	- 新生代与老年代不再是物理隔离，他们都是Region的集合
2. 建立可预测的停顿时间模型
	- 有计划的避免在整个JAVA堆中进行全区域的垃圾收集(使用Remember Set避免全堆扫描)
	- 可以跟踪Region垃圾价值，优先回收最大的Region

###具体实现
![C1回收器](http://askingwindy-gitcafe.qiniudn.com/并发回收.jpg)

堆内存被划分为多个大小相等的 heap 区, 每个heap区都是逻辑上连续的一段内存 (virtual memory)。其中一部分区域被当成老一代收集器相同的角色 (eden, survivor, old), 但每个角色的区域个数都不是固定的。这在内存使用上提供了更多的灵活性

1. 标记阶段完成后,G1就可以知道哪些heap区的empty空间最大。它会首先回收这些区,通常会得到大量的自由空间——这也是为什么这种垃圾收集方法叫做Garbage-First(垃圾优先)的原因
2. 顾名思义, G1将精力集中放在可能布满可收回对象的区域, 可回收对象(reclaimable objects)也就是所谓的垃圾.
3. G1使用暂停预测模型(pause prediction model)来达到用户定义的目标暂停时间,并根据目标暂停时间来选择此次进行垃圾回收的heap区域数量.

###优点
紧凑的空闲内存区间且没有很长的GC停顿时间
