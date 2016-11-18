# **cephfs相关**

# 文件系统cephfs部署前置条件

* 相应主机配置好主机名，如node1, node2, node3
* 相应主机配置好ip
* 相应主机配置好dns,目前使用静态dns: /etc/hosts
* 相应主机创建好cluster账号，cluster账号互信任，且具有sudo权限并不要密码
* hacluster账号的码??????

# 文件系统挂载

* 配置ntp, 使用chrony
* 提供inventory文件
* 执行playbook: cephfs-init.yml

ntp: chrony
cephfs
redis
rabbitmq
lvs

# 文件系统初始状态
挂载点: /var/shtermdata/share
初始目录：shterm(user=sherm, group=shterm)和redis(user=redis, group=redis)
共享的文件需要存放到shterm目录，如/var/shtermdata/share/shterm/bsscripts,/var/shtermdata/share/shterm/guisesslog
初始拷贝主节点的/var/shtermdata/share/shterm目录到cephfs

# cephfs-init.yml inventory文件格式
    [admin-node]
    node1 ip=1.1.1.1  //操作节点: 格式: <hostname> ip=<ip>

    [nodes]
    node1 ip=1.1.1.1  //集群节点：格式: <hostname> ip=<ip>
    node2 ip=1.1.1.2
    node3 ip=1.1.1.3

# cephfs-rm.yml inventory文件格式
remove结点关机后
    [admin-node]
    node1 ip=1.1.1.1

    [nodes]
    node1 ip=1.1.1.1
    node2 ip=1.1.1.2

    [remove]
    node3 ip=1.1.1.3 osd_num=2

# cephfs-add.yml inventory文件格式
    [admin-node]
    node1 ip=1.1.1.1

    [nodes]
    node1 ip=1.1.1.1
    node2 ip=1.1.1.2

    [add]
    node3 ip=1.1.1.3
