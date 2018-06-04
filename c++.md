# C++98

# C++03

# C++11

### std::move vs std::forward

### constexpr

### 智能指针
unique_ptr, share_ptr, weak_ptr
autoptr(deprecated)

### shared_ptr vs make_shared

### 统一初始化
C++11之前，C++主要有以下几种初始化方式：

	//小括号初始化
	string str("hello");

	//等号初始化
	string str="hello";

	//大括号初始化
	struct Studnet{
	    char* name;
	    int age;
	};
	Studnet s={"dablelv",18}; //纯数据（Plain of Data,POD）类型对象
	Studnet sArr[]={{"dablelv",18},{"tommy",19}};  //POD数组

虽然C++03提供了多样的对象初始化方式， 但不能提供自定义类型对象的大括号初始化方式，也不能在使用new[]的时候初始化POD 数组。幸好，C++11扩充了大括号初始化功能，弥补了C++03的不足。

	class Test{    
	    int a;    
	    int b;    
	public:    
	    C(int i, int j);    
	};    
	Test t{0,0};                    //C++11 only，相当于 Test t(0,0);    
	Test* pT=new Test{1,2};         //C++11 only，相当于 Test* pT=new Test(1,2);    
	int* a = new int[3]{ 1, 2, 0 }; //C++11 only


此外，C++11大括号初始化还可以应用于容器，终于可以摆脱 push_back() 调用了，C++11中可以直观地初始化容器了：

	// C++11 container initializer    
	vector<string> vs={ "first", "second", "third"};    
	map<string,string> singers ={ {"Lady Gaga", "+1 (212) 555-7890"},{"Beyonce Knowles", "+1 (212) 555-0987"}}; 

因此，可以将C++11提供的大括号初始化作为统一的初始化方式，既降低了记忆难度，也提高的代码的统一度。

此外，C++11中，类的数据成员在申明时可以直接赋予一个默认值：

	class C    
	{
	private:  
	    int a=7; //C++11 only
	};    

### 范围 for 循环
值/引用

### unique_lock vs lock_guard

# C++14
### unique_ptr vs make_unique

# C++17

# overwrite and final



# Erase–remove idiom
[http://carnal0wnage.attackresearch.com/]

# 临时对象
   