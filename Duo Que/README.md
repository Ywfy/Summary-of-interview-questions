# 多益面经题
## 1、http和https的区别
```
HTTP协议传输的数据都是未加密的，也就是明文的，因此使用HTTP协议传输隐私信息非常不安全，为了保证这些隐私数据能加密传输，
于是网景公司设计了SSL（Secure Sockets Layer）协议用于对HTTP协议传输的数据进行加密，从而就诞生了HTTPS。
简单来说，HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。

　　HTTPS和HTTP的区别主要如下：

　　1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

　　2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

　　3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

　　4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
```

## 2、sql注入
如何发生
```
对于select * from table where username = XX and password = XX
若是我们对应username的输入框输入' or 1 = 1 #
就会导致执行时忽略#后面的内容，即以上语句执行时变成
select * from table where username = ' or 1 = 1
```

如何避免
```
1、采用预编译语句PreparedStatement
2、使用正则表达式过滤传入的参数
3、输入字符串过滤
```

## 3、string和stringBuffer和StringBuilder的区别
```
String为字符串常量，而StringBuilder和StringBuffer均为字符串变量，即String对象一旦创建之后该对象是不可更改的，
但后两者的对象是变量，是可以更改的。
在线程安全上，StringBuilder是线程不安全的，而StringBuffer是线程安全的
```

## 4、JVM内存模型
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Duo%20Que/Image/JVM%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B.png)<br>

## 5、Java内存泄漏和内存溢出
```
内存溢出 out of memory，是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；

内存泄露 memory leak，是指程序在申请内存后，无法释放已申请的内存空间，内存泄露堆积后果很严重，无论多少内存,迟早会被占光。

memory leak会最终会导致out of memory！
```

## 6、gc机制
[JVM垃圾回收机制](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Other/JVM_GC.md#jvm%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6)<br>

## 7、HashMap
```
HashMap类实现了一个静态内部类Node，Node有三个成员--key，value，next key和value显然就是存储键值对的内容，
next则是指向下一个Node的引用

HashMap内部其实就是哈希表的链地址法的实现,具体采用哈希表和链表以及红黑树三种结构(当哈希冲突到达一定程度后，链表转为红黑树)
HashMap不是线程安全的，通过以下语句保证同步(即线程安全)
Map m = Collections.synchronizedMap(new HashMap(...)); 
```
## 8、reentranlock和synchronized的区别
```
1、线程A和B都要获取对象O的锁定，假设A获取了对象O锁，B将等待A释放对O的锁定，

     如果使用 synchronized ，如果A不释放，B将一直等下去，不能被中断

     如果 使用ReentrantLock，如果A不释放，可以使B在等待了足够长的时间以后，中断等待，而干别的事情
     
2、synchronized是在JVM层面上实现的，不但可以通过一些监控工具监控synchronized的锁定，而且在代码执行时出现异常，
JVM会自动释放锁定，但是使用Lock则不行，lock是通过代码实现的，要保证锁定一定会被释放，就必须将unLock()放到finally{}中

3、在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，
Synchronized的性能会下降几十倍，但是ReetrantLock的性能能维持常态；
```

## 9、数据库索引
https://blog.csdn.net/sinat_39587248/article/details/80521068

## 10、数据库的内连接和外连接
```
内连接 SELECT  XXX FROM XXX INNER JOIN XXX ON XXX (inner可以省略)
对于内连接，只返回两张表都满足ON后面条件的数据

外连接分为左连接和右连接
左连接 SELECT XXX FROM XXX LEFT JOIN XXX ON XXX
右连接 SELECT XXX FROM XXX Right JOIN XXX ON XXX
左连接返回左表的全部数据，但是右表只返回满足ON后面条件的数据
右连接返回右表的全部数据，但是左表只返回满足ON后面条件的数据
```

## 11、间隙锁
```
对Innodb表单，事务隔离级别为可重复读，查询索引为普通索引,手动提交事务的条件下
对同一表单进行并发访问时，一个终端若执行了删改 或者是 查询+for update，就会导致间隙锁
间隙锁实际上锁的是一个区间，比如有一张表，普通索引列a有[2,4,6,8]，
这边执行了delete from tableName where a = 4;
另外一边执行insert into tableName values(3),重点是普通索引a插入一个3，Enter后会卡住也就是被阻塞
因为间隙锁锁住的不单单是4，它还把4跟前后行的空隙锁住了，也就是[2,4]和[4,6)锁住了
```

## 12、进程和线程的区别
```
进程是资源分配的最小单位，线程是CPU调度的最小单位。

进程有自己的独立地址空间，相互不影响。线程只是进程的不同执行路径

线程没有独立的地址空间，多进程的程序比多线程程序健壮

进程的切换比线程的切换开销大
```

## 13、进程的通信方式
```
进程有五种通信方式:
1、管道
2、FIFO(命名管道)
3、消息队列
4、信号量
5、共享内存

1. 管道pipe：管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。
2. 命名管道FIFO：有名管道也是半双工的通信方式，但是它允许无亲缘关系进程间的通信。
3. 消息队列MessageQueue：消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。
4. 共享存储SharedMemory：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。
5. 信号量Semaphore：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。
```

## 14、cookie和session的区别
```
Session是保存在服务器上的数据结构，用于跟踪用户的状态。此数据可以保存在群集、数据库、文件中。

Cookie是客户端存储用户信息的机制。它用于记录有关用户的一些信息，是实现会话的一种方式。
```

## 15、数据库三大范式
https://baijiahao.baidu.com/s?id=1591955163343123446&wfr=spider&for=pc
```
第一范式：每一列都是不可分割的原子数据项
第二范式：要求实体的属性完全依赖于主关键字。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性
第三范式：任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）
```

## 16、SQL中char和varchar的区别
```
char是定长的，也就是当你输入的字符小于你指定的数目时，char(8)，你输入的字符小于8时，它会再后面补空值。
当你输入的字符大于指定的数时，它会截取超出的字符。

长度为 n 个字节的可变长度且非 Unicode 的字符数据。n 必须是一个介于 1 和 8,000 之间的数值。
存储大小为输入数据的字节的实际长度，而不是 n 个字节。所输入的数据字符长度可以为零。
```

## 17、TCP和UDP的区别
```
1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付
3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。
4.每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
5、TCP对系统资源要求较多，UDP对系统资源要求较少。
```

## 18、数据库四操作语句
```
增 insert into tableName values(xx,xx,xx)
删 delete from tableName where xx = xx
改 update tableName set xx = xx where xx = xx
查 select xx from tableName
```

## 19、MySQL四种事务隔离级别
|事务隔离级别|脏读|不可重复读|幻读|
|:---|:---|:---|:---|
|读未提交(read-uncommitted)| 是 | 是 | 是|
|不可重复读(read-committed)| 否 | 是 | 是|
|可重复读(repeatable-read)| 否 | 否 | 是|
|串行化(serializable)| 否 | 否 | 否|

## 20、堆栈溢出
```
堆栈溢出的产生是由于过多的函数调用，导致调用堆栈无法容纳这些调用的返回地址，一般在递归中产生。
堆栈溢出很可能由无限递归产生，但也可能仅仅是过多的堆栈层级。
```

## 21、全局变量的优缺点
```
全局变量优点：
1.全局可视，任何一个函数都可以访问和更改变量值。
2.内存地址固定，读写效率高
缺点：
1.容易造成命名冲突
2.当值不正确或者出错时，很难确定是哪个函数更改过这个变量
3.线程不安全
```

## 22、TCP的三次握手和四次挥手
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Duo%20Que/Image/%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.JPEG)<br>
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Duo%20Que/Image/%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B.JPEG)<br>
```
SYN，ACK，FIN存放在TCP的标志位，一共有6个字符，这里就介绍这三个：

SYN：代表请求创建连接，所以在三次握手中前两次要SYN=1，表示这两次用于建立连接，至于第三次什么用，在疑问三里解答。

FIN：表示请求关闭连接，在四次分手时，我们发现FIN发了两遍。这是因为TCP的连接是双向的，所以一次FIN只能关闭一个方向。

ACK：代表确认接受，从上面可以发现，不管是三次握手还是四次分手，在回应的时候都会加上ACK=1，表示消息接收到了，
并且在建立连接以后的发送数据时，都需加上ACK=1,来表示数据接收成功。

seq:序列号，什么意思呢？当发送一个数据时，数据是被拆成多个数据包来发送，序列号就是对每个数据包进行编号，
这样接受方才能对数据包进行再次拼接。

初始序列号是随机生成的，这样不一样的数据拆包解包就不会连接错了。
（例如：两个数据都被拆成1，2，3和一个数据是1，2，3一个是101，102，103，很明显后者不会连接错误）

ack:这个代表下一个数据包的编号，这也就是为什么第二请求时，ack是seq+1，
```

```
流程：
在创建连接时，

1.客户端首先要SYN=1,表示要创建连接，

2.服务端接收到后，要告诉客户端：我接受到了！所以加个ACK=1，就变成了ACK=1,SYN=1

3.理论上这时就创建连接成功了，但是要防止一个意外（见疑问三），所以客户端要再发一个消息给服务端确认一下，这时只需要ACK=1就行了。

三次握手完成！

在四次分手时，

1.首先客户端请求关闭客户端到服务端方向的连接，这时客户端就要发送一个FIN=1，表示要关闭一个方向的连接（见上面四次分手的图）

2.服务端接收到后是需要确认一下的，所以返回了一个ACK=1

3.这时只关闭了一个方向，另一个方向也需要关闭，所以服务端也向客户端发了一个FIN=1 ACK=1

4.客户端接收到后发送ACK=1，表示接受成功

四次分手完成！
```

```
为什么需要三次握手?
答：可能出现的问题：如果一个连接请求在网络中跑的慢，超时了，这时客户端会重发请求，
但是这个跑的慢的请求最后还是跑到了，然后服务端就接收了两个连接请求，然后全部回应就会创建两个连接，浪费资源！

如果加了第三次客户端确认，客户端在接受到一个服务端连接确认请求后，后面再接收到的连接确认请求就可以抛弃不管了。
```

```
为什么需要四次分手?
TCP是双向的，所以需要在两个方向分别关闭，每个方向的关闭又需要请求和确认，所以一共就4次。
```

## MySQL 去除重复记录留下ID值最大的记录
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Duo%20Que/Image/%E9%99%A4%E9%87%8D1.png)<br>
```
DELETE 
FROM
	test
WHERE
	id not in (
		SELECT
			dt.maxid
		FROM
			(
				SELECT
					MAX(id) AS maxid
				FROM
					test
				GROUP BY
					a
			) dt
	)
```
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/Duo%20Que/Image/%E9%99%A4%E9%87%8D2.png)<br>
