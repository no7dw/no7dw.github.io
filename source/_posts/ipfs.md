### IPFS 探索

比特币当前是用于存金融交易数据，有leveldb 存关键小的交易数据。那么我们的文件，譬如一个网站里面的static file 怎么办？

IPFS（InterPlanetary File System）是一个点对点的分布式超媒体分发协议，它整合了过去几年最好的分布式系统思路，为所有人提供全球统一的可寻址空间，包括Git、自证明文件系统SFS、BitTorrent和DHT，同时也被认为是最有可能取代HTTP的新一代互联网协议。

先来看看例子：
https://ipfs.io/ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG


#### 启动：
    ifs daemon
    ![daemon](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/770ee8ad-4c6e-4f48-a0a5-9195d2f425a6.png)



http://localhost:5001/webui

peers

 ![peers](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/fdfc58ab-4b9c-4eb2-8373-f9fad8e63547.png)


耗费网络的ipfs 节点
![network-usage](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/9bf33608-fc78-4cd1-bc9f-db1f7db6c09b.png)

#### 基本操作：
* 添加文件：
    * ipfs add file path
    * ![add file](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/da7539c4-f792-4c10-9200-6666be4346b5.png)
* 获取
    * ipfs cat /ipfs/hash  
    * ipfs cat /ipfs/QmVg7JXmiZy8YRXMKS2VvmXzmPAgRo7KQvajsuVjtGfcB3 > 1.pdf
    * web 方式
    * ![web](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/6570d24c-0fe9-4e52-938e-b028493f0671.png)
    * wget https://ipfs.io/ipfs/QmciDozPmgVpRKNuuGtvT72o1BExSKE7SWFWczvyMfmM4d
    * ![wget](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/f6cede87-644d-42b4-8419-acbfb77ebff2.png)

#### 空间问题

* pin
* 那么删除呢？
    * https://github.com/ipfs/faq/issues/9  
    * https://discuss.ipfs.io/t/how-can-i-delete-a-file-from-ipfs/1556 https://stackoverflow.com/questions/43118022/how-do-i-unpin-and-remove-all-ipfs-content-from-my-machine
    * ipfs pin rm $YOUR_HASH
    * ipfs repo gc
    * 你只能删你本地的数据，你关不了别的节点，尤其是别的节点的pin 数据
![rm](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/5944a978-0b39-4762-a81f-d0fad6920358.png)

##### GC
GC打出的log ，这里面包含其他的节点的别人的数据，我还是能在本地看到这些数据，
![gc](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/b4e0d583-a8e1-499f-9ad7-5b2c9cacda22.png)
递归形：除了直接的内容访问外，还有一种特殊的，recursive
![recursive](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/a581b227-b6ed-46e4-a35e-7d69b0035649.png)


担心硬盘爆：
* config 路径: ~/.ipfs/config 
* Datastore.StorageMax 默认10GB 限制max storage
* StorageGCWatermark:90  90%存储空间使用了的时候触发GC

因为存储空间有限，不是每个人都自愿无偿的贡献存储空间的，所以数据不保证永久存储，想保证永久，要pin serivce
（https://docs.ipfs.io/guides/concepts/pinning/） ，以避免重要的存储数据被触发delete 掉。

进一步了解关于pin机制： https://discuss.ipfs.io/t/trying-to-better-understand-the-pinning-concept/754/2
总结：
* pin的内容会告诉自己的节点不要进入GC 删除他
* 对pin的操作控制不同步到其他节点，其他人的节点爱咋地咋地
* 自己的节点add的，会自动pin （Objects added through ipfs add are pinned recursively by default.）
* 如果都pin了，应该是根据访问量、陈旧度来决定GC

#### ipfs vs BitTorrent 几大区别点
* BT 依赖的是torrent 文件， ipfs 仅通过hash 找到文件
* ipfs 不仅仅支持文件的下载，日后是一种完善的文件协议
* 参考git支持，通过hash 避免重复数据的存储，区分不同的版本 
* 对于web 应用来说，ipfs 是一种cache system
https://medium.com/@kidinamoto/ipfs-vs-bittorrent-9f1c3adb8fcd
https://github.com/ipfs/notes/issues/208
https://github.com/ipfs/faq/issues/17


#### suck part:
*  ipfs add file 后， 后缀都会丢失
* 有时 ，下载很慢 slow / cannot download 
* $ wget https://ipfs.io/ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc
--2018-10-22 12:15:00-- https://ipfs.io/ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc
Resolving ipfs.io (ipfs.io)... 209.94.78.81, 209.94.90.1, 209.94.78.80, ...
Connecting to ipfs.io (ipfs.io)|209.94.78.81|:443... connected.
HTTP request sent, awaiting response...
Read error (Connection timed out) in headers.
Retrying.

--2018-10-22 12:30:01-- (try: 2) https://ipfs.io/ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc
Connecting to ipfs.io (ipfs.io)|209.94.78.81|:443... connected.
HTTP request sent, awaiting response...

 但 official资源比较快，如：
https://ipfs.io/ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG 
* 
猜想：可能需要与是否永久存储的问题相关
* 你关闭了节点daemon 后一段时间(1天以上)，再访问自己的资源，发现不能访问；启动回daemon，1分钟内又可以访问了；再紧接着断开daemon ，disable local cache of chrome ，也还是能访问;后续又停了后，又不能访问了。 — 应该是ipfs.io  帮助cache 了，但没多少节点真正原意pin 并记录，可能要不在search，要不就找不到记录的节点。— 这个部分要深入看代码才知道（可能相关访问频率、访问时间的cache 机制在控制）。
* 相对于传统cdn无权限访问控制

#### 其他
注意通过浏览器获取回来还是经过了ipfs.io 的服务器，背后应该是经过gateway ，然后通过ipfs node 获取回来

more need to dive into：
* public key usage 
* 对于可变内容， 参考：https://docs.ipfs.io/guides/concepts/ipns/ 
* IPFS 未来要支持Namecoin ，那么代表传统DNS、ICANN 在网络中的工作角色会被干掉/替换。
* FileCoin 对比
    * Storj
    * Fcoin
    * Ulord
    * Burst

### FAQ ：

是否add file 就永久存储：
* content storage not forever , then who decide long persistence --> Filecoin http://www.infoq.com/cn/articles/how-ipfs-is-disrupting-the-web 
* IPFS doesn't solve the persistence problem for you, the only way currently to ensure that your files will exist is to pin them on an IPFS node, which means you need pin rights on that node. Run your own node, there are a few services out there that you can pay to pin content, find a node that will volunteer to pin your content, or wait for Filecoin which solves the problem by allowing you to pay Filecoin for persistence.

激励机制：
* 符合存储证明的获得token奖励
    * 反欺诈机制（防止只存一段时间就删）：
        * 隔断时间检查文件是否存在
            * 太频繁---导致消耗资源
            * 太少频率--导致欺诈概率上升
            * 
Ref：
http://liyuechun.org/2017/09/18/ipfs-blockchain/
