# 关于多线程的面试题
##使用Runnalbe还是Thread?
JAVA里面允许调用多个接口，但是不允许多继承，所以实现Runable接口更好

##Runnable和Callable有什么不同？
- Runnable和Callable都代表那些要在不同的线程中执行的任务

- Callable是在JDK1.5增加的。它们的主要区别是Callable的 call() 方法可以返回值和抛出异常（返回Future对象）

##Thread类中的start() 和 run() 方法有什么区别？
- start()方法被用来启动新创建的线程，而且start()内部调用了run()方法
- 当你调用run()方法的时候，只会是在**原来的线程**中调用，没有新的线程启动，start()方法才会启动新线程