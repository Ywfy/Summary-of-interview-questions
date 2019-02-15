# JVM垃圾回收机制

JVM体系结构概览<br>
![图片无法加载](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Other/JVM%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E6%A6%82%E8%A7%88.png)<br>
垃圾回收GC发生会Heap(堆)<br>

## JVM采用分代收集算法
现在的JVM采用的是分代收集算法，根据对象的存活周期的不同而将内存分为几块，分别为新生代、老年代和永久代。
* 新生代(Young区)：朝生夕灭的对象（例如：方法的局部变量等） 频繁回收 Minor GC
* 老年代(Old)：存活得比较久，但还是要死的对象（例如：缓存对象、单例对象等) 较少回收 Full GC
* 永久代(Perm)：对象生成后几乎不灭的对象（例如：加载过的类信息） 无回收

## GC四大算法
### 引用计数法
给对象中添加一个引用计数器，每当有一个地方引用它时，计数器值就加1；当引用失效时，计数器值就减1；任何时刻计数器为0的对象就是不可能再被使用的。

缺点：
* 每次对对象赋值时均要维护引用计数器，且计数器本身也有一定的消耗
* 较难处理循环引用
* <strong>JVM的实现一般不采用这种方式</strong>

### 复制算法(Copying)
* 年轻代中使用的是Minor GC，这种GC算法采用的是复制算法(Copying)
* 将原有的内存空间分为两块，每次只使用其中一块，在垃圾回收时，将正在使用的内存中的存活对象复制到未使用的内存块中，之后，清除正在使用的内存块中的所有对象，交换两个内存的角色，完成垃圾回收。

![图片无法加载](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Other/cop.png)<br>

优点：
* 没有标记和清除的过程，效率高
* 没有内存碎片，可以利用bump-the-pointer实现快速内存分配

缺点：
* 需要双倍空间，造成空间的极大浪费

### 标记清除(Mark-Sweep)
* 老年代一般是由标记清除或者是标记清除与标记整理的混合实现

![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Other/bz.png)<br>

### 标记压缩(Mark-Compact)
* 发生在老年代

![图片无法加载](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Other/bjzl.png)<br>
在整理压缩阶段，不再对标记的对象做回收，而是通过所有存活对象都向一端移动，然后直接清除边界以外的内存<br>
