title: 创业公司如何做好数据驱动的开发工作
date: 2018-07-03 23:57:35
tags:
---
创业公司数据体系建设
# 创业公司如何做好数据驱动的开发工作

创业公司钱、开发资源有限，考究更少的工作产生更大的价值，更快的迭代mvp。
data -> info -> knowledge -> wisdom

尽量减少拍脑袋的决策。
决策要从过往经验到数据驱动；当没有经验的时候，参考外部、常识、少量测试验证。

数据驱动，不仅仅是采集数据，取数的效率，数据的质量，对数据的验证都是非常关键的。

## 报表数据



### 轻量数据

  系统架构复杂度决定采用的方式

  - 单体应用，单个应用DB：直接从应用的DB的副本集读取，为防止报表数据的读写影响主系统。
    副本集根据线上应用DB压力、报表DB压力情况选择：

    - 可以直接从DB集群中挑取只读DB来做报表操作
    - 可以通过同步机制(oplog/binlog)，同步另外的集群去操作。

  - 跨系统/微服务应用：

    - 通过调用微服务的api来获取数据，缺点：大量数据操作的性能消耗应为来回的消耗在调用方与DB之间，数据操作慢。
    - 通过数据同步机制，同步多个DB源到一个报表DB（HBase/MongoDB）。


### BI 报表  
  
  由于负责报表的开发的一般是熟悉 SQL/R/Python，所以考虑直接SQL类的数据直接查到时最合适的（投入时间少、熟悉度高）。[!img]图
  BI 报表我们可以选择类似Redash/SuperSet 这类工具，来快速定制业务的报表。


### 数据分析系统建立的阶段

stage 1：   有效利用第三方统计平台
  baidu/google
  umeng
  漏斗、留存、热点、bug、网络、用户的画像(自己也要分析)

例如通过推广活动热点数据，可以发现有些用户体验（UX）上，设计与实际有用户逾期有误。


stage 2:
  热点、漏斗、行为
  fullstory/appsee/mouseflow
  GrowingIO/诸葛IO 

stage 3:
  建立自己的数据分析平台

基本漏斗：访问、注册、下载、交易、复投
常见的业务指标：
获客、留存率
生命周期：流失型、成长型、新用户
金融的指标：标签，欺诈分数（自定义），价值分数（自定义）


### 系统的指标监控

几个需要关注的维度
  - Nginx 
  - APP Log  
  - DB Log 报表
  - ELK 报表
    - 定制自己的业务应用系统维度

  - Grafana 报表

    维度：


  - 机器的性能
  - 容器的性能

  异常报警

### 数据可供业务方访问

  物理部署给报表DB到业务方
  小量: excel/csv 导出，方便分析
  BI自助：提供模板BI自取
  大量：API SDK 调用方式，方便Python/R分析

excel ，自己lookup
界面自定义查询
提供一定的sql，开发、业务方自主到查询
提供一定的data sdk ，开发、业务方自主到查询


### AB test
  
  金融公司，模型指标，不要猜测，去证实。
  工具:
    - ab test（https://github.com/xavimb/ab-testing）[!img]图
    - apphoc

### 数据质量
全公司的事:
防止错误数据进入prod
业务方与数据开发的同理配合


### 作为开发，需要关心业务

  读懂业务的指标 ：
  普通（DRU、DAU）
  专业（ROC、AUC、GINI）
  数据全栈工程师

### 需求

  需求避免拍脑袋。

理想的情况下，除了有各种的报表维度，对数据可以导出or在线热查询，以便业务人员自己解决自己的需求。



### 迭代的效果回顾

  ROI！！！
