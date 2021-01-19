### tableau教程 快速入门

#### 先基础了解你的数据

-  先选数据源
    -  了解你的数据源
    -  字段含义、字段类型
    -  数据更新频率、数据量、波动频率范围
        -  决定了你的时间颗粒度
        -  例子：
            -  页面转化率（约小时级别+日级别）
            -  财务营收收（日+周+月级别）
        -  细粒度只看最近的x小时/天/周
-  想清楚你的需求、定义、dash 大概样子
-  如果需要多表看，则join 数据, 了解下方的各种join 区别
    -  原理注意：MongoDB 经BI connector unwind后，把数据独立一个表
-  项目组内的架构介绍

    ![2021-01-19-11-09-39](http://img.no1token.com/2021-01-19-11-09-39.png)
    -  tableau client/server <-> (cached)SQL data source (dremio) <-> BI connector  <-> synced MongoDB(NoSQL) @China   <-> MongoShake    <-> MongoDB(Prod Secondary)  @Foreign County
        -  another : tableau client/server <-> (cached)SQL data source (dremio) <-> hive(hadoop)  <-> DB Secondary <-> DB router / Syncer <-> DB (Prod Secondary)
    -  web browser <-> (cached) tableau server <-> (cached)SQL data source (dremio)
    -  了解这个有助于找问题、debug
    -   

#### 基础图形

柱状-group
-  string group 
    ![2020-04-05-15-59-26](https://imgs.no1token.com/2020-04-05-15-59-26.png)
-  0/1  sum /count中
    ![2020-04-05-15-59-45](https://imgs.no1token.com/2020-04-05-15-59-45.png) 

-  bin 分箱 sum/count
    ![2020-04-05-16-00-01](https://imgs.no1token.com/2020-04-05-16-00-01.png) 
    ![2020-04-05-16-00-56](https://imgs.no1token.com/2020-04-05-16-00-56.png)


-  time - series 
    ![2020-04-05-16-01-18](https://imgs.no1token.com/2020-04-05-16-01-18.png)

-  multiple  line 
   ![2020-04-05-16-01-43](https://imgs.no1token.com/2020-04-05-16-01-43.png)
-  different calculate method: accumulate(累计)、 PCT change(波动)
   ![2020-04-05-16-02-18](https://imgs.no1token.com/2020-04-05-16-02-18.png) 

A
#### advance part 

Performance issue：
-  fiter in datasource:
    ![2020-04-05-16-03-42](https://imgs.no1token.com/2020-04-05-16-03-42.png)
-  Fiters/ Pages/Format/Marks/Parameters  in dash
-  join 区别
    -  https://stackoverflow.com/questions/5706437/whats-the-difference-between-inner-join-left-join-right-join-and-full-join 
    ![2020-04-05-16-03-59](https://imgs.no1token.com/2020-04-05-16-03-59.png)
-  create function
    -  例如增加一个比率计算 
    ![2020-04-05-16-04-29](https://imgs.no1token.com/2020-04-05-16-04-29.png)
    -  多行报表
    ![2020-04-05-16-04-44](https://imgs.no1token.com/2020-04-05-16-04-44.png) 



-  其他注意事项：
    -  不要随便拿表来join ， 见上文：看数据量、更新频率、波动频率
    -  线下折腾，不要随便搞线上
    -  不要随便publish，注意权限，探索的数据尽量不要上传到tableau server
-  没有指标 不要做决定（unless you got good six sense）
-  一图胜千言


教学资源：
-  https://www.youtube.com/results?search_query=tableau+tutorial
-  https://www.youtube.com/playlist?list=PLWPirh4EWFpGXTBu8ldLZGJCUeTMBpJFK (推荐)
-  https://community.tableau.com/welcome
-  google.com

#### 优化
-  tableau 可以做很多dash，所以很多时候各种dash 都可以做，甚至监控也可以
    -  所以很多时候 管理后台的各种dash 统计、图是不需要在code 了
    -  监控粒度为每小时
-  查询优化
    -  查询中dremio缓存：
        -  转化后的sql 执行在dremio
        ![2020-04-05-16-05-28](https://imgs.no1token.com/2020-04-05-16-05-28.png)
    -   没cache 
        -  转化后的sql 执行在dremio—> MongoDB
        -  转化后的sql 执行在dremio—> hive
        ![2020-04-05-16-05-56](https://imgs.no1token.com/2020-04-05-16-05-56.png)

#### tricks

有时由于数据量过大，tableau 分析会有比较慢、数据库有压力，建立增加筛选，从少量数据建立验证后再扩大到大的数据集
- 从数据的时间维度筛选
- tableau做数据提取
- 抽样（id 去尾号是x）的
- 从业务维度增加筛选条件