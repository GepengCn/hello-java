# MySQL

### MySQL怎么解决死锁

!!! tip "MySQL怎么解决死锁"

大多数数据库有两种方式解决：

- 死锁检测机制
- 死锁超时机制

MySQL的InnoDB用的死锁检测，会将持有最少行排他锁的事务进行回滚

### MySQL事务特性

!!! tip "MySQL事务特性"

- 默认自动提交
- 非事务型的表（用的非事务性的存储引擎），变更无法撤销，即无法回滚
- 隐式锁和显示锁
    - 隐式锁：事务执行过程根据隔离级别随时加锁，commit和rollback时释放
    - 显示锁：select ... lock in share mode或select .. for update
    
    
### MVCC

!!! tip "MVCC"

保存数据在某个时间点的快照来实现的。如果当前行要被修改或删除，读操作不会去等锁释放，而是读取行的快照数据（历史数据）

- 怎么实现的：每行记录后保存两个隐藏的列，保存了创建和过期的时间，这个时间不是实际的时间而是版本号，每开始一个事务，版本号会递增。读的时候会根据版本号判断是否可见，不可见去获取快照数据。


### 事物的四大特性(ACID)介绍一下？

!!! tip "事物的四大特性(ACID)介绍一下？"


- 原子性： 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；

- 一致性： 执行事务前后，数据保持一致，多个事务对同一个数据读取的结果是相同的；

- 隔离性： 并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；

- 持久性： 一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。


### 并发事务带来哪些问题？

!!! tip "并发事务带来哪些问题？"

- 脏读（Dirty read）: 当一个事务正在访问数据并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问了这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操作可能是不正确的。

- 丢失修改（Lost to modify）: 指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。例如：事务1读取某表中的数据A=20，事务2也读取A=20，事务1修改A=A-1，事务2也修改A=A-1，最终结果A=19，事务1的修改被丢失。

- 不可重复读（Unrepeatableread）: 指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

- 幻读（Phantom read）: 幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。


### 不可重复度和幻读区别：

!!! tip "不可重复度和幻读区别"

不可重复读的重点是修改，幻读的重点在于新增或者删除。


### MySQL高性能优化有什么规范建议？

!!! tip "MySQL高性能优化有什么规范建议？"

- [x] 数据库基本设计规范

1. 所有表必须使用Innodb存储引擎
2. 数据库和表的字符集统一使用UTF8
3. 所有表和字段都需要添加注释
4. 尽量控制单表数据量的大小，建议控制在500万以内。
5. 禁止在表中建立预留字段
6. 禁止在数据库中存储图片，文件等大的二进制数据
7. 禁止在线上做数据库压力测试
8. 禁止从开发环境，测试环境直接连接生产环境数据库

- [x] 索引规范

