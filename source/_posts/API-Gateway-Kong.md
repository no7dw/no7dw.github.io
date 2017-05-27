title: API Gateway:Kong 
date: 2017-05-26 23:58:06
tags:
---
kong
### what

###  problems
![before](https://getkong.org/assets/images/homepage/diagram-left.png)
![architecture](http://oqln5pzeb.bkt.clouddn.com/17-5-27/16371874.jpg)
多个服务要写自己的log，auth，对于比较耗时的，有时还要高流量限制。

### solution intro
![new](https://getkong.org/assets/images/homepage/diagram-right.png)

单点部署的情况：
![spof](http://oqln5pzeb.bkt.clouddn.com/17-5-27/25167726.jpg)
![spof scale out](http://oqln5pzeb.bkt.clouddn.com/17-5-27/34167398.jpg)



### why not just haproxy log (kinbana)
haproxy rate limit http://blog.serverfault.com/2010/08/26/1016491873/
simple version:

```
  frontend fe_api_ssl
    
    acl too_many_uploads_by_user sc0_gpc0_rate() gt 100
    acl mark_seen sc0_inc_gpc0 gt 0
   
    stick-table type string size 100k store gpc0_rate(60s)
   
    tcp-request content track-sc0 hdr(Authorization) if METH_POST document_request is_upload
   
    use_backend be_429_slow_down if mark_seen too_many_uploads_by_user  
   
  backend be_429_slow_down
    timeout tarpit 2s
    errorfile 500 /etc/haproxy/errorfiles/429.http
    http-request tarpit


  backend be_api

```

###  feature
 - logs
 - rate-limit
 - auth 
 - monitoring
[more plugin](https://getkong.org/plugins/)

### install
 try to use docker instead of pkg/deb/vagrant

    docker run -d --name kong-database  -p 5432:5432   -e "POSTGRES_USER=kong"   -e "POSTGRES_DB=kong"   postgres:9.4
    docker run -d --name kong-database  -p 9042:9042  cassandra:3


    dengwei@RMBAP:~/projects/github/kong$ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS                                                                                                      NAMES
    b1b969345f2c        kong:latest         "/docker-entrypoin..."   16 hours ago        Up 16 hours                0.0.0.0:7946->7946/tcp, 0.0.0.0:8000-8001->8000-8001/tcp, 0.0.0.0:8443->8443/tcp, 0.0.0.0:7946->7946/udp   kong
    9d73317da8e3        cassandra:3         "/docker-entrypoin..."   16 hours ago        Up 16 hours                7000-7001/tcp, 7199/tcp, 9160/tcp, 0.0.0.0:9042->9042/tcp                                                  kong-database
                                                 kong-database

### config
    
    http localhost:8001

```
HTTP/1.1 200 OK
...
Server: kong/0.10.2

{
    "configuration": {
        "admin_ip": "0.0.0.0",
        "admin_listen": "0.0.0.0:8001",
        "admin_listen_ssl": "0.0.0.0:8444",
        "admin_port": 8001,
        "admin_ssl": true,
        ...
        "admin_ssl_ip": "0.0.0.0",
        "admin_ssl_port": 8444,
        "anonymous_reports": true,
        "cassandra_consistency": "ONE",
        "cassandra_contact_points": [
            "kong-database"
        ],
        "cassandra_data_centers": [
            "dc1:2",
            "dc2:3"
        ],
        "cassandra_keyspace": "kong",
        "cassandra_lb_policy": "RoundRobin",
        "cassandra_port": 9042,
        ...
        "pg_user": "kong",
        "plugins": {
            "acl": true,
            ...
        },
        "prefix": "/usr/local/kong",
        "proxy_ip": "0.0.0.0",
        "proxy_listen": "0.0.0.0:8000",
        ...
    },
    "hostname": "b1b969345f2c",
    "lua_version": "LuaJIT 2.1.0-beta2",
    "plugins": {
        "available_on_server": {
            "acl": true,
           ...
        },
        "enabled_in_cluster": {}
    },
   ...
    "tagline": "Welcome to kong",
    "timers": {
        "pending": 4,
        "running": 0
    },
    "version": "0.10.2"
}
```

#### adding an api:

    http POST localhost:8001/apis name=demo upstream_url=http://mockbin.org/request request_host=mockbin.org

host with port

    http POST localhost:8001/apis name=localdemo upstream_url=http://localhost:3010/request hosts=localhost

#### list apis:

    http localhost:8001/apis

#### check admin log
in docker container:

    sh-4.2# ls
    access.log  admin_access.log  error.log  serf.log
#### use plugin

 - auth example 

        http POST localhost:8001/apis/0ee4b228-3089-4ae9-b13a-09ba4df8004e/plugins name=key-auth config.key_names=X-AUTH
        http POST localhost:8001/consumers/b7199b84-cbe6-47ef-9cd0-c68ab27dfee0/key-auth key=abc123

verify :

    http localhost:8000 HOST:mockbin.org X-AUTH:1234
    http localhost:8000 HOST:mockbin.org X-AUTH:abc123
previous one won't work , latter one works, which with the right **key**

- rate limit example:
find your api id by list apis

    http localhost:8001/apis

in my example   the api id is:  0ee4b228-3089-4ae9-b13a-09ba4df8004e 

    http POST localhost:8001/apis/0ee4b228-3089-4ae9-b13a-09ba4df8004e/plugin;5Cs name=rate-limiting config.minute=5 config.hour=10

test it:

    http localhost:8000 Host:mockbin.org X-AUTH:abc123
    HTTP/1.1 200 OK

after 5 times with 1 minute:

    dengwei@RMBAP:~/projects/work$  http localhost:8000 Host:mockbin.org X-AUTH:abc123
    HTTP/1.1 429
    Connection: keep-alive
    Content-Type: application/json; charset=utf-8
    Date: Thu, 25 May 2017 12:18:35 GMT
    Server: kong/0.10.2
    Transfer-Encoding: chunked
    X-RateLimit-Limit-hour: 10
    X-RateLimit-Limit-minute: 5
    X-RateLimit-Remaining-hour: 0
    X-RateLimit-Remaining-minute: 5
    
    {
        "message": "API rate limit exceeded"
    }

### how does it work

rest api  with other url in sub page
ui for monitor(need enterprise)
plugin with other language?


### to do or not
[api gateway: to be or not to be](https://www.slideshare.net/saltynut/api-gateway-to-be-or-not-to-be)


### ref
[API & Microservices Management with Kong](https://www.youtube.com/watch?v=S6CeWL2qvl4)
[kong基础使用](https://yq.aliyun.com/articles/63180)
[kong ui](https://github.com/rajarju/kong-ui.git)
[kong dashboard](https://github.com/PGBI/kong-dashboard)
[docker](https://github.com/Mashape/docker-kong/blob/master/compose/docker-compose.yml)
[使用Kong来管理业务restful api](https://my.oschina.net/u/2600078/blog/781724)
[聊聊架构：深入浅出聊聊企业级API网关](https://mp.weixin.qq.com/s?__biz=MzA5Nzc4OTA1Mw==&mid=2659599286&idx=1&sn=f41c9dc7f9f2027eab97889b1b01a391&chksm=8be996a4bc9e1fb29ea77d0941bedb60714c6a7ae94edd44bf705a0910979e18e631210ab326&scene=0&key=836b3b965f94a78ba80fb048666939a2e7de5f5c01f7fbc167a17d3ddd10220531b77bd6e5c8c5ff87643c6d9c5ace0cdb12721aa629d55b250738c6842327e6fff205bd24f060498c7f84e1597959cd&ascene=0&uin=MjQ2NTA3MzgwMg%3D%3D&devicetype=iMac+MacBookPro12%2C1+OSX+OSX+10.12.4+build(16E195)&version=12020510&nettype=WIFI&fontScale=100&pass_ticket=%2F6qDX3DsRvTP%2FN295fYWIZ9sSlykZxBYcayxgbeiqSen6vZ5YEJ%2F%2BCqIWEtL7z0J)

### problems
in docker you will not success in forward your request via kong. [issue here](https://github.com/Mashape/kong/issues/182)

        dengwei@RMBAP:~/projects/work$ http POST localhost:8001/apis name=localdemoabc upstream_url=http://localhost:3010/ uris=/abc

        HTTP/1.1 201 Created

        dengwei@RMBAP:~/projects/work$ http localhost:8000/abc host=localhost
        HTTP/1.1 502 Bad Gateway

### todo:
nginx + koa sample
how routing work and verify
ui page
speed lost
comparing with other api gateway: loopback.io http://orange.sumory.com/


