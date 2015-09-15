# 类加载器：双亲委派模型与OSGi
##类如何在虚拟机中确定唯一性？
1. 类本身
2. 类加载器（每一个类都拥有一个独立的类名称空间）

##类加载器包括
### Bootstrap ClassLoader
加载JACA_HOME\lib，或者被-Xbootclasspath参数限定的类
-  不可扩展，不可以被程序员引用

###Extension ClassLoader
加载\lib\ext，或者被java.ext.dirs系统变量指定的类
###Application ClassLoader
ClassPath中的类库

##双亲委派模型
![双亲委派模型图](http://askingwindy-gitcafe.qiniudn.com/类加载器.png)

除了顶层的Bootstrap ClassLoader外，每一个类都拥有父类记载其
- 父子关系由**组合**（不是继承）来实现

### 工作流程
1. 类加载器收到类加载请求，首先将请求委派给父类加载器完成（所有加载请求最终会委派到顶层的Bootstrap ClassLoader加载器中）
2. 如果父类加载器无法完成这个加载（该加载器的搜索范围中没有找到对应的类），子类尝试自己加载（直接委派到Bootstrp ClassLoader中）

###好处
1. 优先级的层次关系
2. 避免同一个类被多次加载

##OSGi加载器
###什么是OSGi
OSGi是动态模块系统，它为模块化应用的开发定义了一个基础架构
###OSGI的优点
####从开发角度来看：
1. 基于OSGI的组件模型bundle能隐藏内部的实现，而bundle是基于服务的交换

####从部署与运行角度：
1. 可以动态更新（热插拔特性），bundle可以动的安装、启动、停止、更新、卸载，系统无需重启

####从类加载角度上看：
2. 每一个bundle有自己的类加载器，当需要更换bundle时，连同类加载器一起换掉
3. 不再是双亲委派的树桩结构，而是网状结构
4. 没有固定的委派模型，只有具体使用某个package或者class时，根据package的导入导出的定义来构造bundle之间的委派和依赖

###OSGi加载
####OSGi加载器的划分
![OSGI网状类加载架构](http://askingwindy-gitcafe.qiniudn.com/OSGI类加载.png)
1. 父类加载器：JAVA提供，包括BC、EC、AC，用于加载java.*开头的类以及在父类委派清单中声明要委派给父类加载器加载的类
2. Bundle类加载器：每个bundle独立的加载器，当一个bundle去请求加载另外一个bundle导出的package中的类时，需要把加载请求委派给导出类的那个bundle区处理，而自己无法加载其他bundle的类
3. 其他类加载器：框架类加载器等

#### 一个Bundle的名称空间组成
1. 父类加载器提供的类(java.*开头的类 & 委派名单中列明的类)
2. Import-Package
3. Require-Bundle
4. 本bundle的classpath(私有package, bundle-classpath)
5. 附加的fragment bundle
6. 动态导入的package

####工作流程简要说明
1. java.*开头的类，委派给父类加载器
2. 否则，委派列表名单的类委派了父类加载器
3. or,import列表中的类，委派给export这个类的bundle去加载
4. or，查找当前bundle的classpath自己加载
5. or,...

###相互依赖的bundle将导致死锁
 BundleA 依赖BundleB，同时B依赖A。加载A时，会锁定当前的实例对象，然后把请求委派给B处理（等待）；加载B时，同样等待A
####解决方法
启用osgi.classloader.singleThreadLoads参数，按照单线程串行方法强行类加载操作，从底层避免死锁，但是降低了性能

##参考
1. [OSGI简介](http://osgi.com.cn/article/7289226)
