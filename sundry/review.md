
## MySQL 相关

#### 1. 索引，事物，间隙锁，mvcc, 聚簇索引
索引：hash索引，全文索引，B+Tree 索引
事物（ACID） 原子性，一致性，隔离性，持久性


#### 2. 事物隔离级别

	READ UNCOMMIT  事务能够看到其他事务没有提交的修改，当另一个事务又回滚了修改后的情况，又被称为脏读dirty read
	READ COMMITED  事务能够看到其他事务提交后的修改，这时会出现一个事务内两次读取数据可能因为其他事务提交的修改导致不一致的情况，称为不可重复读
	REPEATABLE READ	事务在两次读取时读取到的数据的状态是一致的
	SERIALIZABLE   可重复读中可能出现第二次读读到第一次没有读到的数据，也就是被其他事务插入的数据，这种情况称为幻读phantom read, 该级别中不能出现幻读

## Java 基础

#### 1. 线程池，锁

#### 2. Hash ConcurrentHashMap

#### 3. JVM 相关

#### 4. Spring 中的 hash


## Redis 相关

#### 1. 分布式锁

