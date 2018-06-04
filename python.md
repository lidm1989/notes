# 文件编码

# 默认值

dict.get('foo', True)

dict.get('foo') or True

# 内存管理

# 垃圾回收

### 引用计数（reference counting）

### 标记清除（mark and sweep）

### 分代搜集（generation）

# 编译
    python -m compileall <dir>

# 反编译
    pip install uncompyle
    uncompyle6 -o <dir> *.pyc

# virtualenv

# 工具
    python -m SimpleHTTPServer
    python -m json.tool