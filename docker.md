# **安装**
    yum install docker

# 命令
    docker images
    docker run
    docker attach
    docker rmi
    docker inspect
    docker ps
    docker rm
    docker pull [选项] [Docker Registry地址]<仓库名>:<标签>
    docker tag myregistrydomain:port/foo/bar myregistrydomain:port/foo/bar

    docker save
    docker load

    docker export
    docker import

# docker-compose

# docker.service
    Environment="HTTP_PROXY=http://192.168.8.162:3128" "HTTPS_PROXY=https://192.168.8.162:3128"
# /etc/docker/daemon.json
{
    "registry-mirrors": ["http://hub.c.163.com"],
    "live-restore": true,
    "insecure-registries": ["archlinux.com:5000"]
}

# docker registry
    docker run -d -p 5000:5000 --restart=always --name registry -v /data:/var/lib/registry registry:2

# docker registry mirror
It’s currently not possible to mirror another private registry. Only the central Hub can be mirrored.

# docker hub

# docker trusted registry

# minikube
    minikube start --vm-driver=kvm --iso-url=file:///home/lidm/minikube-v1.0.7.iso
