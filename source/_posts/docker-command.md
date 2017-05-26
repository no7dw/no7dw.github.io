title: docker-command
date: 2016-01-14 21:48:22
tags:
---
# docker command line

标签（空格分隔）： docker

---

### list images

    dengwei@dengweis-MacBook-Pro:~$ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    <none>              <none>              266834cd4285        8 weeks ago         183.9 MB
    ubuntu              latest              e9ae3c220b23        9 weeks ago         187.9 MB
    hello-world         latest              0a6ba66e537a        3 months ago        960 B


### list containers

    dengwei@dengweis-MacBook-Pro:~$ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                        PORTS                    NAMES
    3509401563a1        ubuntu              "/bin/bash"              2 weeks ago         Exited (137) 10 minutes ago   0.0.0.0:5000->5000/tcp   sharp_franklin
    c546c7e2122f        266834cd4285        "node"                   2 weeks ago         Exited (0) 2 weeks ago                                 cocky_mcnulty
    0e9b0475adc8        266834cd4285        "node"                   2 weeks ago         Exited (0) 2 weeks ago                                 dreamy_elion

### run container

    dengwei@dengweis-MacBook-Pro:~$ docker run -t -i e9ae3c220b23
    root@5dd4903c0fbb:/# ls   
    
    dengwei@dengweis-MacBook-Pro:~$ docker run -t -i ubuntu:latest
    root@1e330c221b44:/# ls
    
    dengwei@dengweis-MacBook-Pro:~$ docker run  hello-world
    
    Hello from Docker.
    This message shows that your installation appears to be working correctly.

### stop container

    dengwei@dengweis-MacBook-Pro:~$ docker stop c546c7e2122f  0e9b0475adc8 4ea492cb3de8
    c546c7e2122f
    0e9b0475adc8

### restart
    
    $ docker restart my_container

### after you stopped some container, you may now remove it

    dengwei@dengweis-MacBook-Pro:~$ docker rmi c546c7e2122f

### exit container

    ctrl+D
    or
    root@5dd4903c0fbb:/# exit

### search images

    dengwei@dengweis-MacBook-Pro:~$ docker search node
    NAME                       DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
    node                       Node.js is a JavaScript-based platform for...   1530      [OK]       
    nodesource/node                                                            32                   [OK]
    strongloop/node            StrongLoop, Node.js, and tools.                 20                   [OK]


### enter container

    $ docker exec -it <container id> /bin/bash

### Print app output

    $ docker logs <container id>




