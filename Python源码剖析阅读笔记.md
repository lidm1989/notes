# **Python源码剖析阅读笔记**

# scopes 
It is important to realize that scopes are determined textually.
On the other hand, the actual search for names is done dynamically, at run time — however, the language definition is evolving towards static name resolution, at “compile” time, so don’t rely on dynamic name resolution!

# 当进入一个新的名字空间或者说作用域时，就算是进入一个Code Block了

#LGB与LEGB

# 当进入一个新的名字空间或者说作用域时，就算是进入一个Code Block了

# 切片引用与切片赋值
