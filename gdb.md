# gdb

# gdb --tui

# gcore

# file

# show
    show env
    show directories

# args
    set args
    show args
    
# break
    break, tbreak, rbreak

# 常用
    info break
    break
    run
    step
    continue
    finish
    backtrace  # 查看调用栈

# 多进程
    set follow-fork-mode parent
    set follow-fork-mode child

    info inferiors
    inferior <ID>

# 多线程
    info threads  # 查看当前进程的线程
    thread <ID>  # 切换调试的线程为指定ID的线程
    