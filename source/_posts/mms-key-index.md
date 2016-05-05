title: MMS key index
date: 2015-10-15 13:30:26
tags:
---
# MMS关键指标意义&各数值区间意义&部署
## part 1 mms图
------
### What's MMS
MongoDB Management Service (MMS) is a suite of services for managing MongoDB deployments.

### 统计图表的数据来源
all statistics can show in mongo shell by:

    >db.serverStatus()

### 

 1. opcounters
意义：The average number of commands performed per second since the last data point
代表忙不忙，越小越好。（看业务）
![opcounters][1]
 2. queue
 
意义：The number of operations queued waiting for any lock －－ 排队未执行的命令
越小越好，平时<1
![此处输入图片的描述][2]
 3. connection

意义：The number of currently active connections to this server. A stack is allocated per connection; thus very many connections can result in significant RAM usage.
越小越好。
=client (connected) * 集群server 数目
![此处输入图片的描述][3]
 4. lock

意义：The percent of time write locked. The effective lock % is the percent of time in the global lock plus the percent of time locked by the hottest database. Because the data is sampled and combined, it is possible to see values over 100%.
越小越好。
造成锁增加的行为：update delete insert ...
![此处输入图片的描述][4]
 5. Btree

意义：A large number of misses means that you are indexes are too big to fit in RAM, which can cause a significant performance penalty －－ 因为数据不再mem,从disk 加载到mem。
mongodb 使用 btree index
missratio ＝ misses/access 最好是0
access:  The number of times the btree indexes have been accessed. 
hits:The number of times a btree page was in memory
misses: The number of times a btree page was not in memory
注意：不要只看一条线
![此处输入图片的描述][5]
 6. Asserts

意义：The number of regular asserts raised since this process started
"asserts" : {
        "regular" : <num>,
        "warning" : <num>,
        "msg" : <num>,
        "user" : <num>,
        "rollovers" : <num>
},
不要只看一条线
![此处输入图片的描述][6]
 7. Page faults

意义：The number of page faults on this process. In non-Windows environments this is hard page faults only.
越小越好。
![此处输入图片的描述][7]
  ref:[link][8]

## PART 2 mms-monitoring-agent and mms-backup-agent
monitoring agent: 主要用于监控db 状态

backup agent 主要用于备份db 到云端


monitoring:
安装：
https://docs.cloud.mongodb.com/tutorial/install-monitoring-agent-with-deb-package/

如何开启／关闭

    sudo start mongodb-mms-monitoring-agent
    sudo stop mongodb-mms-monitoring-agent 
    https://docs.cloud.mongodb.com/tutorial/start-or-stop-monitoring-agent/

backup :
https://docs.cloud.mongodb.com/tutorial/install-backup-agent-with-deb-package/
how to start & stop

    sudo start mongodb-mms-backup-agent

  或者：
  

    sudo nohup ./mongodb-mms-backup-agent >> backup-agent.log 2>&1 &

注意权限

https://docs.cloud.mongodb.com/tutorial/start-or-stop-backup-agent/

  [1]: http://7xk67t.com1.z0.glb.clouddn.com/mms-opcounters.png
  [2]: http://7xk67t.com1.z0.glb.clouddn.com/mms-queues.png
  [3]: http://7xk67t.com1.z0.glb.clouddn.com/mms-connections.png
  [4]: http://7xk67t.com1.z0.glb.clouddn.com/mms-lock.png
  [5]: http://7xk67t.com1.z0.glb.clouddn.com/mms-btree.png
  [6]: http://7xk67t.com1.z0.glb.clouddn.com/mms-asserts.png
  [7]: http://7xk67t.com1.z0.glb.clouddn.com/mms-pageFaults.png
  [8]: http://www.cnblogs.com/no7dw/archive/2013/02/20/2918372.html

