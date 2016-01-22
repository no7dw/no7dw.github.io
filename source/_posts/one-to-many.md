title: one-to-many
date: 2016-01-22 23:59:21
tags:
---
# mongodb ORM 连表中的查询

---

mongodb 2.x 不提供join功能。
mongodb 3.x 提供了类似join 功能，但有类似的效果： $lookup

这里讲的是通过ORM 实现。

[此处输入链接的描述][1]

在mongoose 中使用的ref，
在waterline 中使用的是model 的 **model** 实现关联

关联：
model/User.json

    module.exports = {
    attributes: {
        username : { type: 'string' },
        orders:{
                collection: 'order',
                via: 'owner'
            },
      }
    };


model/Order.json

    module.exports = {
    attributes: {
        amount : { type: 'float' },
        uid: { type : 'string' },
        owner:{
                model:'user'
            }
      }
    };

具体效果代码见：
[此处输入链接的描述][2]


  [1]: https://github.com/no7dw/One-to-Many
  [2]: https://github.com/no7dw/One-to-Many
