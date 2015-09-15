# GC
比较好的了解GC的博文: http://coolshell.cn/articles/11541.html

## 什么时候GC? 
系统在不可预测的时间调用System.gc()函数的时候；当然可以通过调优，用NewRatio控制newObject和oldObject的比例，用MaxTenuringThreshold 控制进入oldObject的次数，使得oldObject 存储空间延迟达到full gc,从而使得计时器引发gc时间延迟OOM的时间延迟，以延长对象生存期

## GC针对的对象是什么？ 
超出了作用域或引用计数为空的对象；从gc root开始搜索找不到的对象，而且经过一次标记、清理(CMS回收算法)，仍然没有复活的对象。 

## GC做了什么？ 
- 删除不使用的对象，回收内存空间
- 运行默认的finalize