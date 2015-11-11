title: haproxy config new ssl
date: 2015-11-11 16:34:55
tags:
---
# haproxy using SSL with evssl.cn

------

https 快过期，通过原授权网站重新申请续费ssl后，拿到dommain_sha256_cn.zip dommain_sha1_cn.zip

解压后，改zip文件提供apache/nginx/other 等web server 的 

  - 公钥
  - 根公钥
  - 私钥
  - 等

haproxy 属于 for Other Server 的情况.
提供:

  - 1_cross_Intermediate.crt
  - 2_issuer_Intermediate.crt
  - 3_user_dommain.crt
  - 4_user_dommain.key
  - root.crt

haproxy 要的是pem 文件，具体参考：
[How To Implement SSL Termination With HAProxy on Ubuntu 14.04][1]
[How To Set Up Apache with a Free Signed SSL Certificate on a VPS][2]


咨询客服后，才知道产生pem 文件需要有顺序：
公钥--中级根--交叉根--顶级根--私钥
即：
 3_domian.crt-->2_issur.crt-->1_cross.crt-->root.crt-->4_domain.key

cat 对应文件  >  - domain.pem

然后替换线上/etc/ssl/private/domain.pem

那么为什么其他文件夹的文件数是不一样的呢？咨询客服后得知，apache/nginx 里面的是1_root_bundle.crt / 1_domain_bundle.crt ，bundle 顾名思义，是包含多个文件(公钥--中级根)的内容

now reload haproxy, 又能愉快的继续使用https了。
 

  [1]: https://www.digitalocean.com/community/tutorials/how-to-implement-ssl-termination-with-haproxy-on-ubuntu-14-04
  [2]: https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-with-a-free-signed-ssl-certificate-on-a-vps

