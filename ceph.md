# **CEPH**

# 简介
Ceph 独一无二地在一个统一的系统中同时提供了对象、块、和文件存储功能。

# 安装

## 安装epel和ceph源。
国内可以用[网易源](http://mirrors.163.com/)或[阿里源](http://mirrors.aliyun.com/)。

## 安装软件包
    yum install ceph ceph-radosgw ceph-deploy

## 创建部署 Ceph 的用户
ceph-deploy 工具必须以普通用户登录 Ceph 节点，且此用户拥有无密码使用 sudo 的权限，因为它需要在安装软件及配置文件的过程中，不必输入密码。

    useradd -m shterm
    passwd shterm 

    echo "shterm ALL = (root) NOPASSWD:ALL" | tee /etc/sudoers.d/shterm
    chmod 0600 /etc/sudoers.d/shterm
    
正因为 ceph-deploy 不支持输入密码，你必须在管理节点上生成 SSH 密钥并把其公钥分发到各 Ceph 节点。 ceph-deploy 会尝试给初始 monitors 生成 SSH 密钥对。

    su shterm
    ssh-keygen

在 CentOS 和 RHEL 上执行 ceph-deploy 命令时可能会报错。如果你的 Ceph 节点默认设置了 requiretty ，执行 sudo visudo 禁用它，并找到 Defaults requiretty 选项，把它改为 Defaults:ceph !requiretty 或者直接注释掉，这样 ceph-deploy 就可以用之前创建的用户（创建部署 Ceph 的用户 ）连接了。

## 开放端口6789并禁用selinux

# 部署
    mkdir my-cluster
    cd my-cluster

    ceph-deploy new <node>
    ceph-deploy mon create-initial

    sudo mkdir /var/local/osd
    sudo chown ceph:ceph /var/local/osd

    ceph-deploy osd prepare <node>:/var/local/osd
    ceph-deploy osd activate <node>:/var/local/osd

    ceph-deploy admin <node>

    sudo ceph osd pool create cephfs_data 64
    sudo ceph osd pool create cephfs_metadata 64
    sudo ceph fs new cephfs cephfs_metadata cephfs_data

    ceph-deploy mds create <node>

# 命令
    ceph mon dump
    ceph osd dump
    ceph mds dump
    ceph fs dump

# 配置
    osd pool default size = 2
    osd pool default min size = 1

    osd pool default pg num = 64
    osd pool default pgp num = 64

    osd mon report interval max = 5  # OSD 守护进程每 120 秒会向监视器报告其状态，不论是否有值得报告的事件。在 [osd] 段下设置 osd mon report interval max 可更改OSD报告间隔，或运行时更改。
    mon osd report timeout = 30  # 如果一 OSD 在 mon osd report timeout 时间内没向监视器报告过，监视器就认为它 down 了。
    mon osd down out interval = 5  # 在 OSD 停止响应多少秒后把它标记为 down 且 out

    mds reconnect timeout = 10

# 参数
    osd heartbeat interval = 6  # 各 OSD 每 6 秒会与其他 OSD 进行心跳检查，用 [osd] 下的 osd heartbeat interval 可更改此间隔、或运行时更改。
    osd heartbeat grace = 20  # 如果一个 OSD 20 秒都没有心跳，集群就认为它 down 了，用 [osd] 下的 osd heartbeat grace 可更改宽限期、或者运行时更改。

# mnt-cephfs.mount
    [Unit]
    Description=Mount Cephfs
    After=ceph-mon@<node>.service

    [Mount]
    What=<node>:6789:/
    Where=/mnt/cephfs
    Type=ceph
    Options=name=admin,secret=<keyring>,_netdev
    TimeoutSec=90s

    [Install]
    WantedBy=multi-user.target

# CEPH术语

OS: Ceph Object Store

MD: Ceph Metadata Server

RADOS: a reliable, autonomous, distributed object store comprised of self-healing, self-managing, intelligent storage nodes.

# 参考
[1][CEPH官网](http://ceph.com/)

[2][CEPH中文社区](http://ceph.org.cn/)
