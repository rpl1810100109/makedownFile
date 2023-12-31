## 思路

在`CuttingPreWidget`类中的`OnStatue`函数中执行加载预览图的操作，在函数中通过svr读取已经加载的nc程序信息,将信息进行切割保留需要的最新的数据，新建一个`GraphicItem`对象将读取的程序信息放入该对象函数中处理,通过两个静态变量存储上一个点的信息，然后将画笔轨迹存入当前对象指针地址为名字的文件中。

在切换文件时删除之前存储的所有文件。

## 特大文件卡顿甚至卡退的问题

目前程序的本质还是不断的触发svr获取最新的NC程序数据，

由于onStatue函数的触发频次特别高，所以会在堆区产生非常多的`GraphicItem`对象（对于测试文件来说会产生大于一万个`GraphicItem`对象，每个对象只负责一小段轨迹）因此程序不可避免的卡顿、越来越卡。

**解决思路：**

​	更改`addPrePath`函数的触发机制,使一个`GraphicItem`承担多段轨迹。

​	由此有一下思路：	

	1. 在观察中增加新的信号，当加载到一定数量时候通知观察者触发`addPrePath`
 	2. 在`OnStatue`中设置一个触发阈值，但是比较麻烦的是结尾部分如果达不到阈值的处理.

## 使用中发现有时候轨迹显示不完全就不加载了

