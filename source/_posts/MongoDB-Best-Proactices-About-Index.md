### MongoDB 索引的最佳实践

大部分开发者都知道加索引会快。但实际过程中，我们常碰到一些疑问&困难：
 - 我们查询的字段会各种case都有，是不是各个涉及查询的字段都要加索引？
 - 复合索引和单字段怎么选择，都加还是每一个的单个字段就好了？
 - 加索引有没有副作用？
 - 到达什么数据量级的时候应该加索引？
 - 索引都加了，但还是不够快？怎么办？
 
本文尝试去解释索引的基本知识&解答上述的疑问。

#### 索引究竟是什么东西？

大部分开发者接触索引，大概知道索引类似书的目录，你要找到想要的内容，通过目录找到限定的关键字，进而找到对应的章节的pageno，再找到具体的内容。
在数据结构里面，最简单的索引实现类似hashmap，通过关键字key，映射到具体的位置，找到具体的内容。但除了hash的方式，还有多种的方式实现索引。

#### 索引的多种实现方式以及特色

hash / b-tree / b+-tree 
redis HSET / MongoDB&PostgreSQL/ MySQL 

hashmap 
![hashmap](http://www.51code.com/uploads/allimg/190315/4_1453444561.jpg)
一图见b-tree & b+-tree 差别
![b-tree & b+-tree](https://i.stack.imgur.com/l6UyF.png)
 
  - b+-tree 叶子存数据，非叶子存索引，不存数据，叶子间有link
  - b-tree 非叶子可存数据

算法查找复杂度上来说
hash 接近O(1)  
b-tree  O(1)~ O(Log(n))更快的平均查找时间，不稳定的查询时间  
b+ tree  O(Log(n)) 连续数据， 查询的稳定性

至于为何MongoDB 的实现选择b-tree 而非 b+-tree ？
网上多篇文章有阐述，非本文重点。



#### 数据&索引的存储

![storage](https://slideplayer.com/slide/13450455/80/images/18/Ensure+indexes+fit+in+RAM.jpg)
存储， index 尽量存储在内存， data 其次。
索引不是越多越好。只保留必要的内存尽量用在刀刃上。
如果index memory占用较多。



#### 知道索引的实现&存储原理后的思考

 insert/update/delete 会触发rebalance tree -- 所以，增删改数据，索引会触发修改，性能会有损耗
***索引不是越多越好***
既然如此，选择哪些字段作为索引呢？当查询用到这些条件，怎么办？
拿一个最简单的hashmap来讲，为什么复杂度不是O(1)，而是所谓接近 O(1)。因为有key 冲突/重复啊~。DB 去找的时候，key 冲突的数据一大堆的话，还是得轮着继续找。
上面的b-tree  看键(key)的选择也是如此。
因此一个大部分开发经常犯的错就是*** 对没有区分度的key 建索引*** ，例如：很多就只有集中类别的 type/status 的 几十万以上的collection 。通常这种没什么帮助。

#### 复合索引
复合索引不是越多越好



### 学会用explain（）验证分析性能

#### 通过explain看 典型理想的查询

### 通过explain看 典型的慢查询

### 突然的慢

### 优化到头了吗？


 