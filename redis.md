# **Redis的安装与使用**

# 简介
Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

# 安装
Redis安装包包含于shterm仓库中

    yum install redis

# 启动
    systemctl start redis
    systemctl enable redis

# 命令
### monitor
    redis monitor 'foo' | grep

# 配置
Redis默认配置满足使用，不用额外配置。

Redis默认配置监听于**127.0.0.1:6379**

# WATCH/MUTIL/EXEC

# 发布/订阅

# 分布式锁 和 分布式信号量

# 参考
[1][Redis官网](http://redis.io/)

[2][redis-py官网](http://redis-py.readthedocs.io/en/latest/index.html)

