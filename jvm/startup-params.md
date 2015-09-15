# JVM启动参数的意义
##`-verbose`
###用法：
1. `-verbose:class`
2. `-verbose:gc`
3. `-verbose:jni`

###意义：
1. `class`: 将类 加载情况在控制台中输出
2. `gc`：将虚拟机的垃圾回收事件信息打印
3. `jni`：将本地方法调用信息打印

##启动时内存分配
- `-Xms`：设置堆内存初始值
- `-XX:PermSize`：设置非堆内存初始值
- `-Xmx`：设置JAVA 堆内存的最大值， 取决于`-Xms`的设置
- `-Xss`：设置每个线程的Stack大小