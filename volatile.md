# 关于 volatile 关键字
### volatile 关键字的说明
volatile 的作用是作为指令关键字，确保本条指令不会因编译器的优化而省略。精确地说就是，优化器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。

### 问题
    // static volatile bool flag = false;
    static bool flag = false;

    DWORD WINAPI test(PVOID lpParam) {
        while (!flag) {
            // Sleep(10);
        }

        return 0;
    }

    int main(int argc, char **argv)
    {
        HANDLE h = CreateThread(NULL, 0, test, NULL, 0, NULL);
        Sleep(1000);  // 等待 test 线程启动，避免 test 还没启动，flag 已被赋值为 true
        flag = true;
        WaitForSingleObject(h, INFINITE);

        return 0;
    }

1. 上面的代码会正常退出吗？
1. 如果将注释行 `Sleep(10)` 打开会怎么样？
1. 变量 flag 应该使用 volatile 修饰吗？

### 结论
使用 **visual studio 2017**，上面问题的结论是：

1. 编译为 **Debug** 版，进程一直运行，不会退出；编译为 **Release** 版，进程正常退出。
1. 打开后 **Debug** 和 **Release** 版进程均正常退出。
1. 使用 volatile 限定后 **Debug** 和 **Release** 版进程均正常退出。

从上述结论我们大致可以猜到，结论一说明由于 **Release** 版的编译优化，导致进程无法正常退出；结论二和结论三会阻止**Release** 版的编译优化。

### 结论一分析
##### Debug 版本生成的汇编如下：
        while (!flag) {
    00D816FE  movzx       eax,byte ptr [flag (0D8913Ch)]  
    00D81705  test        eax,eax  
    00D81707  jne         test+2Bh (0D8170Bh)  
            // Sleep(10);
        }
    00D81709  jmp         test+1Eh (0D816FEh) 

test 行 eax 明显等于 eax, 所以会执行 jmp 跳转到地址 0D816FEh （其中最后的 h 代表 16 进制），也就是 movzx 行。这样当主线程对 flag 进行更新时，子线程能读到此变化。

##### Release 版本生成的汇编如下：
    00F30FFF  add         byte ptr flag (0F33388h)[eax],ah 
        while (!flag) {
    00F31005  test        al,al  
    00F31007  je          test+5h (0F31005h)  
            // Sleep(10);
        }

由于编译器的优化，会先将 flag 放入 eax 寄存器。然后 test 行 al 明显等于 al, 所以会执行 jmp 跳转到地址 0F31005h （其中最后的 h 代表 16 进制），也就是 test 行。由于子进程先将 flag 读入寄存器，以后直接对寄存器中的值进行测试，所以永远不会读到 flag 的变化，子进行进入死循环。

### 结论二分析
##### Debug 版本生成的汇编如下：
        while (!flag) {
    01171DDE  movzx       eax,byte ptr [flag (0117913Ch)]  
    01171DE5  test        eax,eax  
    01171DE7  jne         test+3Ch (01171DFCh)  
            Sleep(10);
    01171DE9  mov         esi,esp  
    01171DEB  push        0Ah  
    01171DED  call        dword ptr [__imp__Sleep@4 (0117A004h)]  
    01171DF3  cmp         esi,esp  
    01171DF5  call        __RTC_CheckEsp (0117111Dh)  
        }
    01171DFA  jmp         test+1Eh (01171DDEh) 

与结论一 **Debug** 版基本一致。

##### Release 版本生成的汇编如下：
        while (!flag) {
    01391000  cmp         byte ptr [flag (01393388h)],0  
    01391007  jne         test+1Eh (0139101Eh)  
    01391009  push        esi  
    0139100A  mov         esi,dword ptr [__imp__Sleep@4 (01392000h)]  
            Sleep(10);
    01391010  push        0Ah  
    01391012  call        esi  
    01391014  cmp         byte ptr [flag (01393388h)],0  
    0139101B  je          test+10h (01391010h)  
        }

cmp 行每次直接用 0 与内存进行比较，这样当主线程对 flag 进行更新时，子线程能读到此变化。

### 结论三分析
##### Debug 版本生成的汇编如下：
        while (!flag) {
    00D816FE  movzx       eax,byte ptr [flag (0D8913Ch)]  
    00D81705  test        eax,eax  
    00D81707  jne         test+2Bh (0D8170Bh)  
            // Sleep(10);
        }
    00D81709  jmp         test+1Eh (0D816FEh) 

与结论一 **Debug** 版基本一致。

##### Release 版本生成的汇编如下：
        while (!flag) {
    00321000  cmp         byte ptr [flag (0323388h)],0  
    00321007  je          test (0321000h)  
            //Sleep(10);
        }

与结论二 **Release** 版基本一致。

### volatile 的使用场景
##### 场景一
多线程应用中被几个任务共享的变量，如上面示例代码。

##### 场景二
中断服务子程序或信号处理器中会访问到的非自动变量。

##### 场景三
阻止编译器优化。如访问并行设备的硬件寄存器。

    XBYTE[2]=0x55;
    XBYTE[2]=0x56;
    XBYTE[2]=0x57;
    XBYTE[2]=0x58;
对外部硬件而言，上述四条语句分别表示不同的操作，会产生四种不同的动作，但是编译器却会对上述四条语句进行优化，认为只有XBYTE[2]=0x58（即忽略前三条语句，只产生一条机器代码）。如果键入volatile，则编译器会逐一地进行编译并产生相应的机器代码（产生四条代码）。

# 参考
[C 语言关键字](http://en.cppreference.com/w/c/language/volatile)

[C++ 关键字](http://en.cppreference.com/w/cpp/language/cv)

[Visual C++ 对 volatile 的说明](https://docs.microsoft.com/en-us/cpp/cpp/volatile-cpp)

[Windows 核心编程 第 5 版 第 8 章 第 3 节]