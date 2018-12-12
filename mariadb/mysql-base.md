# MySQL 基础知识

1. 关于行级锁（InnoDB引擎）   
   在事物中，只有执行了`UPDATE`、`DELETE`，并且还未提交的时候才会上锁，`INSERT`语句不会上锁。  
   > SQL语句不同，上的锁的范围也不同:    
   * `UPDATE test SET name='aaa' WHERE id = 23`，此SQL有`WHERE`条件，只在选定的记录上加锁，
   * `UPDATE test SET name='aaa' `，此SQL无`WHERE`条件，全表加锁。
