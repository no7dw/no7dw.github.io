# 淘宝营销api

### total: 
 - use appkey & secrect
 - variance naming rule
 - 提供沙箱环境
 - 使用api gateway
 - 使用rest(但返回结果包裹了)


### detail design

taobao.promotion.coupons.get (查询卖家优惠券)
查询卖家已经创建的优惠券，接口返回信息：优惠券ID，面值，创建时间，有效期，使用条件，使用渠道，创建渠道，优惠券总数量

model design:
condition: 订单满多少分才能用这个优惠券，501就是满501分能使用。注意：返回的是“分”，不是“元”
create_channel: 优惠券的创建渠道，自己创建/他人创建
json return :

```
{
    "promotion_coupons_get_response":{
        "total_results":200,
        "coupons":{
            "coupon":[
                {
                    "coupon_id":123456,
                    "denominations":500,
                    "creat_time":"2000-01-01 00:00:00",
                    "end_time":"2000-01-01 00:00:00",
                    "condition":501, 
                    "create_channel":"自己创建"
                }
            ]
        }
    }
}
```

taobao.promotion.limitdiscount.get (限时打折查询)

```
  limit_discount_id
  limit_discount_name
  start_time
  end_time
```

taobao.promotion.limitdiscount.detail.get (限时打折详情查询)
限时打折详情查询。查询出指定限时打折的对应商品记录信息。

```
{
    "promotion_limitdiscount_detail_get_response":{
        "item_discount_detail_list":{
            "limit_discount_detail":[
                {
                    "limit_discount_name":"限时打折1",
                    "start_time":"2000-01-01 00:00:00",
                    "end_time":"2000-01-01 00:00:00",
                    "item_id":4674951,
                    "item_discount":"6.5",
                    "limit_num":3  
                }
            ]
        }
    }
}
```
防御设计：
model design: 
limit_num 每人限购数量，1、2、5、10000(不限)
即便是不限，实际也是一个大数目

tmall.promotion.tip.campaign.modify (天猫营销修改活动) & tmall.promotion.tip.campaign.create (天猫营销创建活动接口)
活动数据可以修改：
model design

```
  campaign_id
  start_time
  campaign_name //活动名称
  desc
  free_post //是否包邮
  end_time
  exclude_area //String [] 不包邮地区
  discount_type //活动优惠方式：PERCENT_OFF-打折，DIRECT_DISCOUNT-减钱，FINAL_PRICE-最终价
```

留意活动的优惠方式

### 留意请求异常

返回实例：

```
{
    "error_response":{
        "code":50,
        "msg":"Remote service error",
        "sub_code":"isv.invalid-parameter",
        "sub_msg":"非法参数"
    }
}
```

错误码有对应的错误描述&解决方案

### query parameter 

标准的分页 and with model 的具体字段
page_number


