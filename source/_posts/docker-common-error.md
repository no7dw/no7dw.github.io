title: docker-common-error
date: 2016-01-14 21:50:30
tags:
---
Non-existing image of running container 

    drm() { docker rm $(docker ps -q -a); }
    dri() { docker rmi $(docker images -q); }
    ddri(){ docker rmi $(docker images -f 'dangling=true' -q); }
