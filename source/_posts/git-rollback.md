title: git rollback
date: 2016-06-23 09:42:34
tags:
---
简单介绍线上的几种情况

 - 有最近一个可用的tag

    #如 git checkout v3.2.1
    git checkout "tagname"

 - 最近没有tag，确定某一个commit是ok 的，直接线上回滚到某个commit 的状态
 
    # 首先看log，确认回到某个commit状态
    git log
    # ok 找到某次commit
    git reset --hard "06b96e4147e4ea5be143e9e2490c88b468c26e94"
    
 - 最近没有tag，在线上/本地有**错误**修改,并且已经提交到远程服务器

    # 首先看log，确认回到某个commit状态
    git log
    # ok 找到某次commit
    git revert "06b96e4147e4ea5be143e9e2490c88b468c26e94"

参考：
[git reset revert][1]


  [1]: http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html

    


