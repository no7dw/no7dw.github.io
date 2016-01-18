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
