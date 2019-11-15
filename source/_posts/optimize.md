## 总结
  
  - 要有记录跑的时长数据
  - 分拆script 跑（当前实际有做）
  - 更换到RDS 跑。2x + 速度。
  - 不使用Temporary table，尤其是可以复用的表
  - 补充些index，以下方的explain 为准
  - 运行长的语句，用explain 再调优（看看是否中index，是否中合理index）
  - 还是分拆script
  - 根据修改涉及原始表、验收表，只跑改动，不要全量跑，最终再全量跑


### 环境优化

  当前docker container 有超100% cpu， cpuquota : 0 是有效的。
  阿里云RDS 4 cores 16G， 8.0 version。速度有2x 提升。 （采用）

### script 要拆 
  
  不能改数据就重跑所有
  现在每次重跑，都相当于clean & 全量构建。

### 减少temp table
  
  5.6 版本以上的时候会碰到错误：
  Error Code: 1786. Statement violates GTID consistency: CREATE TABLE ... SELECT. 0.0081 sec
  参考解决：https://blog.csdn.net/tengxing007/article/details/80942951
  但 分拆两部，crate table a like b ; insert into a select x1,x2,x3 from b; 会有表结构（columns数量）不一样的问题，需要手动建表。
  参考 http://blog.sina.com.cn/s/blog_4cd978f90102xkxi.html

  使用 ：create table a as select x1,x2,x3 from b limit 0;

  create table tmp_lendparticulars2 as 
  SELECT sourceCode,transId,sourceFinancingCode,transType,transMoney,userIdcardHash,transTime
  FROM lendparticulars
  group by sourceCode,transId,transType,userIdcardHash limit 0;

#### 分拆出每个关联表&独立表，独立的可以并行操作，各自独立client session 执行

  temporary table 要手动删除数据，不要drop table。 index 会干掉。--- 也即是尽量别temporary table

  CREATE TEMPORARY TABLE wade_tmp_transact
  SELECT sourceCode,transId,sourceProductCode,finClaimId,transferId,replanId,transType,transMoney,userIdcardHash,transTime
  FROM transact
  GROUP BY sourceCode,transId,sourceProductCode,finClaimId,transferId,replanId,transType,transMoney,userIdcardHash,transTime;

  38296329 row(s) affected Records: 38296329  Duplicates: 0  Warnings: 0  478.337 sec

  #### 类似这些表，没必要 tempory table. 要看改动了啥。

  00:44:13  ALTER TABLE wade_tmp_creditor ADD INDEX index_1(userIdcardHash) 2578699 row(s) affected Records: 2578699  Duplicates: 0  Warnings: 0  73.707 sec

  00:45:26  ALTER TABLE wade_tmp_creditor ADD INDEX index_2(finClaimId) 2578699 row(s) affected Records: 2578699  Duplicates: 0  Warnings: 0  110.180 sec

#### 这些也耗时，只是改数据的话，没必要重建索引。
  00:47:55  ALTER TABLE wade_tmp_transact ADD INDEX index_1(userIdcardHash) 38296329 row(s) affected Records: 38296329  Duplicates: 0  Warnings: 0  1540.890 sec

  01:13:36  ALTER TABLE wade_tmp_transact ADD INDEX index_2(sourceProductCode)  38296329 row(s) affected Records: 38296329  Duplicates: 0  Warnings: 0  1375.422 sec


  单个index超过25mins，类似这个，不涉及业务逻辑的。不需要temp table ,导致 rebuild index 

#### 并行：
  #### add index 耗时，考虑试试 并行，多核。锁粒度：表。


#### 减少创建索引的多次扫描。尤其大表有效，百万的作用不大。待验证：1次有效 1次无效

  实例如下：

  ALTER TABLE wade_tmp_transact ADD INDEX index_1(userIdcardHash),ADD INDEX index_2(sourceProductCode),ADD INDEX index_3(finClaimId);

  Query OK, 0 rows affected (30 min 30.50 sec)



### 删减部分索引
-----
其他待确定，删减部分索引，删减部分复合索引（没区分度的那种 or 其他索引足够区分度的）


### 复合索引原则说明：
  只有index_compound(a,b,c)的情况下 ，原则：最左匹配(btree)
  a 走
  b 不走
  b,c 不走
  b,a,c 走 (sql 会优化成 a and b and c 的顺序)
  a,c,b 走

  有index_compound(a,b,c) and c 的情况下
  b,c 走


### 增加索引：

  -- 关键指标 - 通过投资明细计算
  CREATE TEMPORARY TABLE tmp_lendparticulars
  SELECT sourceCode,transId,sourceFinancingCode,transType,transMoney,userIdcardHash,transTime
  FROM lendparticulars
  group by sourceCode,transId,transType,userIdcardHash;

  跟 index： tmp_lendparticulars 不一样的顺序。todo: 验证是否中索引.-- explain 结果显示中，rows 是全表量级。 --- 因为没where  ，没办法。
  explain result:
  '1', 'SIMPLE', 'lendparticulars', NULL, 'index', 'tmp_lendparticulars', 'tmp_lendparticulars', '2012', NULL, '15022751', '100.00', 'Using index; Using filesort'


  索引有 (省略部分)
    PRIMARY KEY (`transId`),    
    KEY `tmp_lendparticulars` (`sourceCode`,`transId`,`sourceFinancingCode`,`transType`,`transMoney`,`userIdcardHash`,`transTime`),

  SELECT sourceCode,sourcefinancingcode,useridcardhash
  FROM tmp_lendparticulars WHERE transtype = '53'
  GROUP BY sourceCode,sourcefinancingcode,useridcardhash;  
  explain result: 没有索引命中，没有减少行数。
  '1', 'SIMPLE', 'tmp_lendparticulars', NULL, 'ALL', NULL, NULL, NULL, NULL, '14737222', '10.00', 'Using where; Using temporary; Using filesort'
  有索引：sourcefinancingcode,useridcardhash， 但没有(sourceCode,sourcefinancingcode,useridcardhash)
  所以增加索引：
  ALTER TABLE tmp_lendparticulars ADD INDEX index_3(transtype),ADD INDEX index_4(sourceCode,sourcefinancingcode,useridcardhash);

  565 sec !!! 悲催， 但下次用回有好处。 主要是 transtype 欠区分度。 drop index 都耗掉546sec 

  实际只加一个：
  ALTER TABLE tmp_lendparticulars ADD INDEX index_4(sourceCode,sourcefinancingcode,useridcardhash);


  before:
  id, select_type, table, partitions, type, possible_keys, key, key_len, ref, rows, filtered, Extra
  '1', 'SIMPLE', 'tmp_lendparticulars', NULL, 'ALL', NULL, NULL, NULL, NULL, '1000', '10.00', 'Using where; Using temporary'

  after:
  id, select_type, table, partitions, type, possible_keys, key, key_len, ref, rows, filtered, Extra
  '1', 'SIMPLE', 'tmp_lendparticulars', NULL, 'index', 'index_3,index_4', 'index_4', '801', NULL, '1000', '100.00', 'Using where'

  1000 行数据 0.12 sec  减少至 0.086 sec 28%


  ### 另外的例子
  CREATE  TABLE online_invest_uid_lender
  SELECT a.sourceCode,a.sourcefinancingcode,a.useridcardhash,a.invamount AS total_amount, b.amount AS repay_money, a.invamount - if(b.amount IS NOT NULL,b.amount,0) AS amount
  FROM total_invest_uid_lender a LEFT JOIN redemption_lender b
  ON a.sourcefinancingcode = b.sourcefinancingcode AND a.useridcardhash=b.useridcardhash
  WHERE a.invamount > if(b.amount IS NOT NULL,b.amount,0)+0.1;

  所以增加索引 #200w 行
  ALTER TABLE total_invest_uid_lender ADD INDEX index_3(invamount);

  redemption_lender 同理
  ALTER TABLE redemption_lender ADD INDEX index_3(invamount);  

  ### 索引增加是为了下次执行更快，如果类似temprary table 一次性的执行，不顾后续，就忽略ADD INDEX , 因为 ADD INDEX 也会耗时间。

### todo
 对比docker & RDS 配置的差异  


