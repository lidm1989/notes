# **CEPH测试**

# 基本配置
    CPU: 6
    MEM: 16G
    Eth: 1000Mbps * 2
    HDisk 4T

# 网络性能
    iperf -s
    iperf -c <ip>

# iostat/vmstat

# 磁盘性能
    dd if=/dev/zero of=here bs=64M count=64 oflag=direct                          166MB/s
    dd if=/dev/zero of=here bs=64M count=64 oflag=sync                            148MB/s

    dd if=/dev/zero of=here bs=1M count=1024 oflag=direct                         164MB/s
    dd if=/dev/zero of=here bs=1M count=1024 oflag=sync                           147MB/s

    dd if=/dev/zero of=here bs=512k count=1024 oflag=direct                       152MB/s
    dd if=/dev/zero of=here bs=512k count=1024 oflag=sync                         78.0MB/s
    
    dd if=/dev/zero of=here bs=8k count=10240 oflag=direct                        42.4 MB/s
    dd if=/dev/zero of=here bs=8k count=10240 oflag=sync                          7.2 MB/s

# filestore + fuse
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64    oflag=direct     22MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64    oflag=sync       22.5MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024  oflag=direct     4.3MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024  oflag=sync       15.9MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=direct    3.3MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=sync      3.0MB/s

# bluestore + fuse
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64     oflag=direct    18.8MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64     oflag=sync      20.1 MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024   oflag=direct    46.3MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024   oflag=sync      45.5MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=256k count=1024   oflag=direct    19.8MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=256k count=1024   oflag=sync      19.3MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=direct    1.8MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=sync      1.6MB/s

# bluestore + mount
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64     oflag=direct    50.2MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=64M  count=64     oflag=sync      102MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024   oflag=direct    20.7MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=1M   count=1024   oflag=sync      20.7MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=256k count=1024   oflag=direct    19.3MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=256k count=1024   oflag=sync      19.0MB/s

    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=direct    1.6MB/s
    dd if=/dev/zero of=/var/shtermdata/share/here bs=8k   count=10240  oflag=sync      1.5MB/s

# bluestore 部署时支持单个硬盘和单个分区，但重启后单个分区模式失败

# 修改配置
/etc/shterm/shterm-text.conf

    -Xmx8192m

    5700 

# 修改配置
/etc/shterm/shterm-text.conf

    -Xmx8192m

/etc/shterm/shterm.conf

    sshd.sessionRecorderBufferSize=1048576
    sshd.compressPlaybackFile=false

    4800 

# 结论
* bs=64M或bs=1M写 网络跑满
* bs=8k写         I/O跑满

* 副本数基本不影响写入速度

# pool
    ceph osd pool ls  <=> rados lspools

# ceph tell
    ceph tell {daemon-type}.{id or *} injectargs --{name} {value} [--{name} {value}]
    ceph tell osd.0 injectargs --debug-osd 20 --debug-ms 1

    ceph daemon {daemon-type}.{id} config show | less
    ceph daemon osd.0 config show | less

# rados bench

1. 安装iso，修改root密码为shterm。http://192.168.8.8/install/test_resource/shterm_distro/WEBAPP-DISTRO-M-x86_64-20161210-092841.iso
1. 配置网络 ip地址分别为10.10.254.101 102 103
1. 下载安装opc包, make install。http://192.168.8.8/install/test_resource/shterm_install/OPC/opc-3.0.9_20161230.tar
1. 修改/etc/passwd root:x:0:0:root:/root:/bin/bash。reboot
1. yum clean all; yum update shterm2*

字符压力测试

1. vip为10.10.254.104
1. 修改admin的密码为111111
1. 新增业务视图根节点-业务系统节点
1. 新增主机10.10.16.41  10.10.16.27 10.10.16.28 10.10.16.44 10.10.16.46 配置root帐号的密码shterm
1. 权限视图中配置admin用于可以访问上面的资源
1. 登录客户段192.168.4.41   4.42  4.43   4.44  4.45
1. 进入目录cd  /var/testdir/stress-webapp-tui
1. 手工执行ssh admin@10.10.16.254.104   选择一台设备登录登录  5台客户端上重复执行
1. 使用 ./webapp_stress-tui2.py 进行压测

