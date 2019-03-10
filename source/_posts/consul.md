### consul & registrator & consul-template 使用

参考这里的文章：
https://www.jianshu.com/p/a4c04a3eeb57

docker-compose.yml

```
version: '3'

services:
  web:
    image: liberalman/helloworld:latest
    environment:
      SERVICE_80_NAME: my-web-server
      SERVICE_TAGS: backend-1
      MY_HOST: host-1
    ports:
    - "80"

  lb:
    image: liberalman/nginx-consul-template:latest
    hostname: lb
    links:
    - consulserver:consul
    ports:
    - "80:80"

  consulserver:
    image: progrium/consul:latest
    environment:
      SERVICE_TAGS: consul servers
    hostname: consulserver
    ports:
    - "8300"
    - "8400"
    - "8500:8500"
    - "53"
    command: -server -ui-dir /ui -data-dir /tmp/consul -bootstrap-expect 1

  registrator:
    image: gliderlabs/registrator:master
    hostname: registrator
    links:
    - consulserver:consul
    volumes:
    - "/var/run/docker.sock:/tmp/docker.sock"
    command: -internal consul://consul:8500
```

`
  docker-compose up
`

`
  docker-compose up --scale web=3
`
![执行效果](https://wade-blog.oss-cn-shenzhen.aliyuncs.com/nginx.gif)

停掉其中一个container ，看看发生什么事情导致能够检测到节点挂掉，并不转发流量？
关键处：订阅了状态的结果后，动态改变了nginx的配置。效果：

![template](https://wade-blog.oss-cn-shenzhen.aliyuncs.com/consule.gif)

单机的架构、原理如下：
![Alt text](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/1551845679501.png)
or 参考：
![Alt text](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/1551846959382.png)


consul 只是其中一种注册中心的实现，registrator support zk&consule/etcd/skydns2 

### registrator 主动检查

https://github.com/gliderlabs/registrator/blob/master/registrator.go
根据tick 的到来，registrator.go 做几件事情：

![registrator.go](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/1551840835089.png)

 - resync interval 到达，扫描， 同步容器状态 refresh ：
   -  alive 的container，
   -  delete   到consul 
 -  refresh interval 到达，sync :
   -  list 当前的container
   -  移除不健康的container
   -  deregister 不健康的container

 
 看到这里，这种基于registrator的主动检测，解耦服务于注册中心的代码
 
 
[ref](https://medium.com/eleven-labs/consul-service-discovery-and-failure-detection-64b06a5cbce6)

### 分布式的配置

每个宿主机都要配置 registrator ，因为只能同一台宿主机去扫描container 的状态，注意红框处：

![Alt text](http://wade-blog.oss-cn-shenzhen.aliyuncs.com/1551845649823.png)

集群的架构

![architecture](https://www.consul.io/assets/images/consul-arch-420ce04a.png)


### consul vs zk , etcd

https://www.consul.io/intro/vs/zookeeper.html

### vs traditional: nginx + lua + jenkins 
传统的发布模式：
 - 服务需要配置健康检查
 - 添加新的服务节点要运维人为操作下到nginx 分发
 - lua 要配置好蓝绿发布状态的控制
 - 如果传统的配置中心：
   -  服务要配置注册中心 register deregister，耦合
   
而现在，只需要简单的配置增加服务节点，就好了

### websocket/https

ref : 
https://www.jianshu.com/p/a4c04a3eeb57
https://segmentfault.com/a/1190000005026022
https://segmentfault.com/a/1190000013720661
https://segmentfault.com/a/1190000005040914 
https://learn.hashicorp.com/consul/getting-started/checks.html
https://www.consul.io/docs/agent/checks.html
https://my.oschina.net/guol/blog/353101
https://www.jianshu.com/p/f8746b81d65d
https://medium.com/eleven-labs/consul-service-discovery-and-failure-detection-64b06a5cbce6
https://www.cnblogs.com/zhangdk/p/Registrator_reference.html
https://kevinguo.me/2017/09/01/docker-consul-consul-template-registrator-nginx/#nginx-with-consul-template
https://juejin.im/post/5b59d64b6fb9a04fd1602017