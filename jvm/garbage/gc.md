# JVM的GC触发机制
一般把新生代的GC称为minor GC ，把老年代的GC称为 full GC，所谓full gc会先触发一次minor gc，然后在进行老年代的GC
## 触发顺序
1. 首先向eden区申请分配空间，如果空间够，就直接进行分配，否则进行一次Minor GC。
2. Minor GC 首先会对Eden区的对象进行标记，标记出来存活的对象。然后把存活的对象copy到From空间。（标记-复制）
	3. 如果From空间足够，则回收eden区可回收的对象。
	4. 如果from内存空间不够，则把From空间存活的对象复制到To区，
	5. 如果TO区的内存空间也不够的话，则把To区存活的对象复制到老年代。
6. 如果老年代空间也不够（或者达到触发老年年垃圾回收条件的话）则触发一次full GC。

简单方向就是Eden->From->To->Old，如下图所示：
![回收顺序](http://askingwindy-gitcafe.qiniudn.com/JVM堆内空间.png)
##持久带（方法区）的GC
默认是不会对持久带（方法区）进行垃圾回收的，设置参数可回收：`-XX:+CMSClassUnloadingEnabled`
