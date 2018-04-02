# windows服务静默升级

wmic path Win32_PerfFormattedData_PerfProc_Process get idprocess,Name,creatingprocessid,PercentProcessorTime,workingsetprivate

# visual studio

# win32 api

# mfc

# vs编译提示：打不开afxwin.h
* 安装mfc基础类库

# vs编译mfc应用提示：无法解析外部符号_main
* 配置——》链接器-》系统-》子系统：选择窗口子系统

# vs编译生成的exe在xp下提示：不是有效的win32应用程序
* 配置——》常规——》平台工具集：- Windows XP

# mfc中正则要额外安装atl server library，alt的规范与正常规范有些许出入

# 进程虚拟地址空间

# malloc 实现
### HeapAlloc VirtualAlloc

# 在进行Windows的学习过程中，经常看到不同的内存分配方式，例如VirtualAlloc, HeapAlloc, malloc和new。它们之间存在一些差异。
1. VirtualAlloc

PVOID VirtualAlloc(PVOID pvAddress, SIZE_T dwSize, DWORD fdwAllocationType, DWORD fdwProtect)

VirtualAlloc是Windows提供的API，通常用来分配大块的内存。例如如果想在进程A和进程B之间通过共享内存的方式实现通信，可以使用该函数（这也是较常用的情况）。不要用该函数实现通常情况的内存分配。该函数的一个重要特性是可以预定指定地址和大小的虚拟内存空间。例如，希望在进程的地址空间中第50MB的地方分配内存，那么将参数 50*1024*`1024 = 52428800 传递给pvAddress，将需要的内存大小传递给dwSize。如果系统有足够大的闲置区域能满足请求，则系统会将该块区域预订下来并返回预订内存的基地址，否则返回NULL。

使用VirtualAlloc分配的内存需要使用VirtualFree来释放。

1. HeapAlloc

HeapAlloc是Windows提供的API，在进程初始化的时候，系统会在进程的地址空间中创建1M大小的堆，称为默认堆（Default Heap），该大小为默认值，可以通过/HEAP连接器开关进行修改。用户也可以通过HeapCreate创建额外的堆，堆的使用可以更有效的进行内存管理，避免线程同步的开销以及快速的释放内存等。HeapAlloc用于从堆上分配一个内存块，如果分配成功则返回内存块的地址。HeapAlloc内部会根据请求的大小以及堆的大小来决定具体的实现，例如在需要大的内存空间时，会自动调用VirtualAlloc函数分配空间。该函数通常用来分配一般大小的内存空间，一些Windows API可能会要求使用该函数进行内存分配并传递给API参数。注意，在分配大的内存块时（例如1M或者更多）最好避免使用堆函数，建议使用VirtualAlloc。

使用HeapFree释放由HeapAlloc的分配的内存。

1. malloc

C语言的内存分配函数，用于分配一般的内存空间，该函数分配的内存不会自动进行初始化。如果使用C语言编程，使用该函数。在Visual C++ 中，malloc函数会调用HeapAlloc函数。

malloc分配的内存由free函数释放。

1. new

C++语言的实现方式，在Visual C++ 中，通过调用HeapAlloc实现内存分配，如果使用C++编程，建议使用new进行一般内存的分配。系统根据调用的方式决定是否对对象进行初始化。

注意： new 在C++中实际上是操作符而不是函数。

使用new 分配的内存由delete / delete[] 进行释放。


# vs生成uac可执行文件
* 配置——》链接器——》清单文件 ——》UAC执行级别：requireAdministrator

# detours

# dll 注入的几种方式
1. AppInit_DLLs方式
1. CreateRemoteThread
1. SetWindowsHookEx
