# **X Window**

# DISPLAY环境变量
X client根据DISPLAY环境变量连接X server。

    DISPLAY=[<host>]:<display>[.<screen>]
* host：运行X server的主机名或IP。
* dispaly：X server监听于6000+dispaly
* screen：显示器

# Xlib
xlib 是 一个用c语言编写的 X Window System 协议的客户端库，
它包含有与x服务器进行通信的函数，编程者可以在不了解x底层协议的情况下直接使用它进行编程。
xlib 初次发表于1985年。使用在类unix系统上。

# glibc
GLibc是GTK+和GNOME工程的基础底层核心程序库，是一个综合用途的实用的轻量级的C程序库，它提供C语言的常用的数据结构的定义、相关的处理函数，有趣而实用的宏，可移植的封装和一些运行时机能，如事件循环、线程、动态调用、对象系统等的API。它能够在类UNIX的操作系统平台（如LINUX， HP-UNIX等），WINDOWS，OS2和BeOS等操作系统台上运行。

# gtk
gtk(gimp toolkit)是一个库，用来写图形用户界面程序。这样的库太多了，windows平台上的mfc等。linux上的Qt、fltk等。

# gtk+
gtk没有采用面向对象机制，消息机制是使用标准的回调机制实现的。 gtk+灵活运用了OOD，消息机制为信号机制。

# pygtk
