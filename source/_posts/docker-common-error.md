title: docker-common-error
date: 2016-01-14 21:50:30
tags:
---
Non-existing image of running container 

    drm() { docker rm $(docker ps -q -a); }
    dri() { docker rmi $(docker images -q); }
    ddri(){ docker rmi $(docker images -f 'dangling=true' -q); }


http://serverfault.com/questions/565294/why-does-a-docker-container-running-a-server-expose-port-to-the-outside-world-ev


can NOT expost port , can not access container from outside

The problem lies in the fact that I ran the container like this:

    docker run -p 3306:3306 asyncfi/magento-mysql

This publishes the container's port to all interfaces of the host machine, which is definitely not what I was looking for at this time. To bind only to localhost, it was necessary to run the container as follows:

    docker run -p 127.0.0.1:3306:3306 asyncfi/magento-mysql

Also the EXPOSE line in Dockerfile is not necessary as the "expose" mechanism is used to link containers.


and in my case , I had to use the same port both outside and inside the container
    docker run -p 127.0.0.1:PORT_A:PORT_B asyncfi/magento-mysql

PORT_A and PORT_B must be the same

runing on OSX:

    dengwei@dengweis-MacBook-Pro:~/docker/node-web-app$ docker info
    Containers: 2
    Images: 59
    Server Version: 1.9.0
    Storage Driver: aufs
     Root Dir: /mnt/sda1/var/lib/docker/aufs
     Backing Filesystem: extfs
     Dirs: 64
     Dirperm1 Supported: true
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 4.1.12-boot2docker
    Operating System: Boot2Docker 1.9.0 (TCL 6.4); master : 16e4a2a - Tue Nov  3 19:49:22 UTC 2015
    CPUs: 1
    Total Memory: 1.956 GiB
    Name: default
    ID: E2AI:WJN3:75JM:65TC:XKGI:GWGN:X22Y:4F6J:5RDS:MXEL:62VH:V6I4
    Debug mode (server): true
     File Descriptors: 53
     Goroutines: 126
     System Time: 2016-01-18T14:04:31.465301917Z
     EventsListeners: 1
     Init SHA1: 
     Init Path: /usr/local/bin/docker
     Docker Root Dir: /mnt/sda1/var/lib/docker
    Labels:
     provider=virtualbox

 
Turn out I should use docker container ip , which is 192.168.99.100 as default, besides you need to make sure the container is running.

![此处输入图片的描述][1]


Dockerfile

error in RUN

    RUN mkdir -p /var/git/finance 
    RUN  cd /var/git/finance 

this will show no such file or folder error
seems RUN in run in parrel, rather in paraller

so we need to change it into this:

    RUN mkdir -p /var/git/finance \
    && cd /var/git/finance \

### network connection issue

    $ docker run hello-world
    Unable to find image 'hello-world:latest' locally
    Pulling repository docker.io/library/hello-world
    Network timed out while trying to connect to https://index.docker.io/v1/repositories/library/hello-world/images. You may want to check your internet connection or if you are behind a proxy.
    bash-3.2$ 

    //fix:
    $ docker-machine restart default      # Restart the environment
    $ eval $(docker-machine env default)  # Refresh your environment settings

### [Cannot connect to the Docker daemon. Is the docker daemon running on this host?](http://stackoverflow.com/questions/21871479/docker-cant-connect-to-docker-daemon) 

    Linux:

    From Create a Docker group section it is neccesary add user to docker group:

    sudo usermod -aG docker $(whoami)
    Log out and log back in. This ensures your user is running with the correct permissions.

    In Mac OSX:

    As Dayel Ostraco says is necessary to add environments variables:

    $ docker-machine start # start virtual machine for docker
    $ docker-machine env  # it's helps to get environment variables
    $ eval "$(docker-machine env default)" #set environment variables
    The docker-machine start outputs the comments to guide the process.


### docker top command error: TERM environment variable not set.

    echo "export TERM=dumb" >> ~/.bashrc

### docker pull 经常碰到网络超时问题
    
    ```
         request canceled (Client.Timeout exceeded while awaiting headers)
    ```

[用国内的加速镜像](https://www.daocloud.io/mirror#accelerator-doc)

    右键点击桌面顶栏的 docker 图标，选择 Preferences ，在 Daemon 标签（Docker 17.03 之前版本为 Advanced 标签）下的 Registry mirrors 列表中加入下面的镜像地址:

```
    http://18fb5052.m.daocloud.io 
```
    点击 Apply & Restart 按钮使设置生效。


 [ref](https://github.com/dockerfile/mariadb/issues/3)


### docker run out of disk causing can't start docker container

error log:

    ```
        Docker Install: Error running DeviceCreate (createPool) dm_task_run failed
    ```
    
solve by remove all images and container:

    ```
        # kill -9 $(lsof -t -c docker)
        # rm -rf /var/lib/docker/*
        # reboot
    ```

[solve link](https://github.com/moby/moby/issues/6325)



  [1]: http://7xk67t.com1.z0.glb.clouddn.com/docker_running_config.png

