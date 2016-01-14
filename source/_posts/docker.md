title: docker
date: 2016-01-14 21:26:31
tags:
---
# Docker

------

build from docker file

nodejs [Dockerfile][1]


dengwei@dengweis-MacBook-Pro:~/docker$ ls
node-0.10
dengwei@dengweis-MacBook-Pro:~/docker$ ls node-0.10/
Dockerfile

dengwei@dengweis-MacBook-Pro:~/docker$ docker build -t nodejs010 ./node-0.10/


  [1]: https://raw.githubusercontent.com/nodejs/docker-node/1e28b4b6a0c2d20469829f70115851ce92ab75c3/0.10/Dockerfile
