# Install to Ubuntu (18.04)

## install docker

```sh
# Update the apt package index:
sudo apt-get update
# Install packages to allow apt to use a repository over HTTPS:
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
# Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
# add repository
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
# install docker ce
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

[reference](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1)
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
docker ps -a # list of all container
docker start ${CONTAINER}
docker attach ${CONTAINER}
```

[about_Dockerfile](http://docs.docker.jp/engine/reference/builder.html)

***

## push to docker

```sh
# login to docker hub
docker login
# tag
docker tag ${image}[:${tag}] ${push_image}[:${tag}]
# push
docker push ${push_image}[:${tag}]
```

***

## Install nvidia-docker2
This is for using cuda-toolkit and cudnn etc.. in docker container.

```sh
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```

--runtime=nvidia: GPU on

[reference](https://github.com/NVIDIA/nvidia-docker)

[nvidia-images](https://hub.docker.com/r/nvidia/cuda/)

[requirements](https://github.com/NVIDIA/nvidia-docker/wiki/CUDA#requirements)
