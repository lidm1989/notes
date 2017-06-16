# 步骤一
通过调节GlyphSupportLevel的级别，可以控制rdp传输文字。再通过OpaqueRect识别出标题栏的位置，从而提取出对应文字。

经测试发现：选项GlyphSupportLevel只对win7，win2008生效。win8，win10，win2012只传输图片。

# 步骤二
通过OpaqueRect识别出标题栏的位置，生成相应图片，再从图片中提取出对应文字。

经测试发现：win8，win10，win2012只传图片，没有OpaqueRect。无法识别标题位置，即便能生成全屏图片也无法提取到标题对应文字。（另外还不知道生成的图片是否完整）

# 步骤三
分析mstsc是否支持传输文字。

经测试发现：Mimikatz工具只能提取到win7、win2008的rdp密钥，无法提取到win8、win10、win2012的rdp密钥。结果只能看到win7、win2008的数据包。

### wireshark编译
[参考](https://github.com/FreeRDP/FreeRDP/wiki/Wireshark-Development)
经测试只有在centos6下编译wireshark-1.4.0版本才能成功编译

### wireshark ssl配置
[参考](https://github.com/FreeRDP/FreeRDP/wiki/Wireshark-Usage)
经测试Mimikatz只能在win7,win2008下获取密钥

# 后续计划
1. 继续尝试调整rdp会话的选项看是否能找到控制rdp传输文字的选项
1. 调查提取win8，win10，win2012的密钥提取方法
1. 调查rdp加密的其它方式，看能不能找到分析mstsc中rdp数据包的方法

# Dockerfile
    FROM centos:6
    WORKDIR /root
    COPY ./wireshark ./wireshark
    #COPY ./FreeRDP ./FreeRDP
    COPY ./FreeRDP-Wireshark ./FreeRDP-Wireshark
    RUN \
        # mirror
        rm -rf /etc/yum.repos.d/* \
        && curl -O http://mirrors.163.com/.help/CentOS6-Base-163.repo \
        && curl -O http://mirrors.aliyun.com/repo/epel-6.repo \
        && mv *.repo /etc/yum.repos.d/ \
        # apps
        && yum install git vim tigervnc-server openbox -y \
        # compile
        && yum install autoconf automake libtool bison flex gtk2-devel libpcap-devel openssl-devel gnutls-devel -y