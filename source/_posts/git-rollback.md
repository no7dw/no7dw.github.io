### git rollback

date: 2016-06-23 09:42:34
tags:
---
简单介绍线上的几种情况

 - 有最近一个可用的tag 

    //如 git checkout v3.2.1
    git checkout "tagname"

 - 最近没有tag，确定某一个commit是ok 的，直接线上回滚到某个commit 的状态 -- 这种是紧急的处理方法。

 
    //首先看log，确认回到某个commit状态
    git log 
    # ok 找到某次commit
    git reset --hard "06b96e4147e4ea5be143e9e2490c88b468c26e94"
        
 - 最近没有tag，在线上/本地有**错误**修改,并且已经提交到远程服务器
 
注意此种情况代表：错误代码已经提交到远程了 —-  所以其他人pull 下来的已经是错误的代码了。现在需要同步回滚所有人最新远程的版本。
一般情况：git revert 在本地操作，线上git pull。

    //首先看log，确认回到某个commit状态
    git log
    # ok 找到某次commit
    git revert "06b96e4147e4ea5be143e9e2490c88b468c26e94"

### 总计：
**如果你只在你自己本地提交了错误代码，并没有提交这部分错误代码到远程，否则需要git revert。
但在线上紧急情况，可临时git reset 。**

参考：
[git reset revert][1]


  [1]: http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html