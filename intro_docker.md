# Install to Ubuntu (18.04)

## install docker
```sh
sudo apt install docker.io
```

***

## change permission of docker "to run without sudo" 
```sh
# check permission
cat /etc/group | grep docker

# add user
sudo gpasswd -a ${USER} docker

# conform
cat /etc/group | grep docker

# change permission
sudo chmod 666 /var/run/docker.sock
```
[reference](https://qiita.com/iganari/items/fe4889943f22fd63692a)

***

## pull image
```sh
docker pull ubuntu:18.04
# docker pull ${IMAGE}:${TAG}
```
[official image](https://github.com/docker-library/official-images/tree/master/library)

***

## build from Dockerfile
```sh
# in directory that has Dockerfile
docker build -t ${IMAGE}[:${TAG}] ./
#share folder hostdir1:hostside, ctdir1:container side
docker run -v /hostdir1:/root/ctdir1 -it --name ${CONTAINER} ${IMAGE} /bin/bash
# after working in your container 
exit
# 
docker ps -a # list of all container
docker start ${CONTAINER}
docker attach ${CONTAINER}
```

[about_Dockerfile](http://docs.docker.jp/engine/reference/builder.html)
