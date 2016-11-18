# **pacemaker**

# 安装
    yum install pacemaker pcs

## 配置root用户的信任关系
    ssh-keygen
    ssh-copy-id

## 开放端口并禁用selinux

## 启动pcs服务
    systemctl start pcsd.service
    systemctl enable pcsd.service

## 配置hacluster用户
    passwd hacluster

## 配置corosync
    pcs cluster auth node1 node2
    pcs cluster setup --name mycluster node1 node2
    pcs cluster start --all
    pcs property set stonith-enabled=false
    pcs resource defaults resource-stickiness=100

## 创建资源
    pcs resource create ceph-mds-node systemd:ceph-mds-node


