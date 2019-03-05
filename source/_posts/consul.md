### consul usage

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

[architecture](https://www.consul.io/assets/images/consul-arch-420ce04a.png)

### 平滑过渡,监控检查

### vs zk , etcd

### vs tradictional: nginx + lua + jenkins 

### websocket/https

