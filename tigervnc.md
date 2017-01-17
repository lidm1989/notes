# **tigervnc**

# 编译环境的安装

[](https://www.x.org/archive//individual/xserver/)

# fltk-1.3.3
    yum install gcc gcc-c++
    yum install libX11-devel
    ./configure --prefix=/usr --libdir=/usr/lib64 --enable-shared
    make
    make install

# tigervnc
    yum install cmake zlib-devel libjpeg-turbo-devel libXtst-devel libXdamage-devel
    mkdir build && cd build
    cmake -G "Unix Makefiles" ..

# xorg
    yum install autoconf automake patch
    yum install xorg-x11-server-devel xorg-x11-font-utils xorg-x11-xtrans-devel libgcrypt-devel libxkbfile-devel libXfont-devel pam-devel
    修改配置

# 运行
    yum install xkeyboard-config  xorg-x11-xkb-utils