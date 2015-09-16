# 线程安全
##线程安全强度
###1. 不可变
- final修饰、String、枚举类、java.lang.Number的部分子类（Long、Double、BigInter）是不可变的
- AtomicInteger等是不可变的

###2. 绝对线程安全
###3. 相对线程安全
Vector、HashTable、Collections等
###4. 线程兼容
HashMap等，它们自身不是线程安全的
###5. 线程对立
Thread里面的suspend()与resume()，如果同时发生会造成死锁
##线程安全实现方法

###非阻塞同步
- 先行操作，如果没有竞争则成功
-如果产生冲突，进行补偿（不断尝试直到成功）

###互斥同步
1. synchronized关键字
	- 锁实例方法：对象的实例加锁
	- 锁类方法(static等属于类的方法)：class加锁

2. ReenTrantLock

3. Lock

####锁优化
1. 适应性自旋
2. 锁消除
3. 锁粗化
4. 轻量级锁
5. 偏向锁

####synchronized与ReentrantLock之间区别
ReentrantLock增加了：等待可中断、可实现公平锁、锁可以绑定