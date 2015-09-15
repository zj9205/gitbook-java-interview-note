# JVM内存划分
![JVM内存划分](http://askingwindy-gitcafe.qiniudn.com/JVM内存管理.png)
## 线程隔离数据区
### 程序计数器 (Program Counter Register)
当前线程所执行的字节码行号指示器

通过改变计数器的值来选取**下一条需要执行的字节码指令**：分支、循环、跳转、异常处理、线程恢复
### JVM虚拟机栈 (Java Virtual Machine Stacks)
Java方法执行内存模型

用于**存储局部变量**、**操作数栈**、动态链接、方法出口等信息
### 本地方法栈 (Native Method Stacks)
为JVM用到本地方法服务
## 线程共享数据区
###方法区(Method Area)，即Nod-Heap,永久代
用于存储JVM加载的**类信息**、**长隆**、**静态变量**以及编译器编译后 的代码等数据

方法区里面有个运行时的常量池
####常量池
用来存放编译器生成的各种**符号引用**和**String常量**

它具备动态性，如果String类调用inter()方法，那么这个字面量将会被放入常量池中

### JVM堆(Java Virtual Machine Heap)，即GC堆
- 垃圾回收主要地方
- 存放所有对象实例的对象
- JVM堆分为新生代与老生代

![JVM堆分为新生代与老生代](http://askingwindy-gitcafe.qiniudn.com/JVM堆内空间.png)

####新生代
由Eden Space和两个大小相同的Survivor组成
####老生代
存放经过多次垃圾回收依然存活的对象
##直接内存
它并不是虚拟机运行时数据区的一部分，也不是JAVA虚拟机规范中定义的内存区域

在JDK1.4中加入了NIO类，引入了一种基于通道(Channel)于缓冲区(Buffer)的I/O方式，它可以使用Native函数库直接分配堆外内存，然后通过一个存储在JAVA堆里面的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在JAVA堆中和Native堆中来回复制数据