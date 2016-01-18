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