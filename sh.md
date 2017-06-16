# **linux常用命令的使用**

# script与scriptreplay


# sudo的配置
    lidm All=(ALL) bin
    用户 主机 切换用户 命令

#  Replacement for netstat is ss.  Replacement for netstat -r is ip route.  Replacement for netstat -i is ip -s link.  Replacement for netstat -g is ip maddr.

# ps
    ps axf
    ps auxH

# python
    python -m SimpleHTTPServer
    python -m json.tool
    
# nc
    nc -l 80 -k -e /bin/hostname
    nc <host> <port>

# brctl/bridge
    
# nmap
    nmap -p <port> <host>
    nmap -n --host-timeout 2 -p 77 -sU 1.1.1.1

# tcp状态
![tcp状态图](C:\Users\qizhi\Desktop\md\tcp.png)

# rpm包解压
    rpm2cpio XXX.rpm | cpio -div

# ssh 
    ssh tunnel

