itle: what-to-consider-when-create-a-new-table
date: 2016-01-29 23:54:18
tags:
---
开一个新表的时候应该要考虑什么事情：

 - 是不是真的需要（废话）
 - 这个表应该会被怎么CRUD-->index 应该怎么建立
 - 以后会有什么变化-->scheme 应该怎么才合理
 - 数据量有多大，增量有多大-->index
 - 应该什么时候建立，要不要建立index，我要怎么存放
 - 是不是主要的服务，还是次要的服务-->我要怎么存放,怎么deploy，次要服务不要影响主要服务
 - 访问的频率、访问的多大 --> db 是否有压力，db 部署架构