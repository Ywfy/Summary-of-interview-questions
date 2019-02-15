# Spring支持的常用数据库事务传播行为和隔离级别

## 常用事务传播行为
Spring通过propagation来指定事务的传播行为<br>
事务的传播行为：即当前的事务方法被另外一个事务方法调用时如何使用事务
  * Propagation.REQUIRED 默认值，使用原来的事务
  * Propagation.REQUIRES_NEW 将原来的事务挂起，开启一个新的事务
  
实际上区别就是若事务出错要回滚，REQUIRED是全部一起回滚，而REQUIRES_NEW则只是回滚出错的那个新事务


## 常用隔离级别
* 脏读（Dirty read）-- 脏读发生在一个事务读取了被另一个事务改写但尚未提交的数据时。如果这些改变在稍后被回滚了，那么第一个事务读取的数据就会是无效的。
* 不可重复读（Nonrepeatable read）-- 不可重复读发生在一个事务执行相同的查询两次或两次以上，但每次查询结果都不相同时。这通常是由于另一个并发事务在两次查询之间更新了数据。
* 幻影读（Phantom reads）-- 幻影读和不可重复读相似。当一个事务（T1）读取几行记录后，另一个并发事务（T2）插入了一些记录时，幻影读就发生了。在后来的查询中，第一个事务（T1）就会发现一些原来没有的额外记录。

|隔离级别|含义|
|:---|:---|
|ISOLATION_DEFAULT|使用后端数据库默认的隔离级别|
|ISOLATION_READ_UNCOMMITTED|允许读取尚未提供的更改。可能导致脏读，幻影读或不可重复读|
|ISOLATION_READ_COMMITTED|允许从已经提交的并发事务读取。可防止脏读，但幻影读和不可重复读仍可能发生|
|ISOLATION_REPEATABLE_READ|对相同字段的多次读取的结果是一致的，除非数据被当前事务本身改变。可防止脏读和不可重复读，但幻影读仍可能发生|
|ISOLATION_SERIALIZABLE|完全服从ACID的隔离级别，确保不发生脏读、不可重复读和幻影读。这在所有隔离级别中也是最慢的，因为它通常是通过完全锁定当前事务所涉及的数据表来完成的|

常用的隔离级别为ISOLATION_REPEATABLE_READ(MySQL默认)和ISOLATION_READ_COMMITTED(Oracle默认)
