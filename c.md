# c89(c90)

### offsetof
    #define offsetof( type, member ) ( (size_t) &( ( (type *) 0 )->member ) )

### __FILE__ __LINE__

# c99

### intx_t uintx_t PRIdx SCNdx

### _Bool vs bool

### int8_t int_fast8_t int_least8_t intmax_t intptr_t

### __func__

# c11

### _Atomic vs volitate
不要将 volitate 用于线程同步

### _Static_assert vs static_assert

### _Alignas vs _Alignof

1. 性能方面，由于是 static_assert 编译期间断言，不生成目标代码，因此 static_assert 不会造成任何运行期性能损失；

1. assert 是运行期断言，它用来发现运行期间的错误，不能提前到编译期发现错误，也不具有强制性，也谈不上改善编译信息的可读性，既然是运行期检查，对性能当然是有影响的，所以经常在发行版本中，assert都会被关掉；

1. #error 可看做预编译期断言，甚至都算不上断言，仅仅能在预编译时显示一个错误信息，它能做的不多，可以参与预编译的条件检查，由于它无法获得编译信息，当然就做不了更进一步分析了。

# gcc扩展

# 结构体
    __attribute__((__packed__))

# 函数
    __attribute__ ((__noreturn__))

# 逗号操作符

# 溢出判断
### 无符号数乘法溢出判断
示例 	glibc calloc

# **system与popen**

# system函数的注入分析
