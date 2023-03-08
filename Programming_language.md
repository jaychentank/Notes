#   C++

```c++
int __builtin_ffs(unsigned int x)
//返回x的最后一位1的是从后向前第几位，比如7368 (1110011001000) 返回4。
int __builtin_clz(unsigned int x)
//返回前导的0的个数。
int __builtin_ctz(unsigned int X)
//返回后面的0个个数，和__builtin_ctz相对。
int __builtin_popcount (unsigned int X)
//返回二进制表示中1的个数。
int __builtin_parity (unsigned int x)
//返回x的奇偶校验位，也就是x的1的个数模2的结果。
//此外，这些函数都有相应的usigned long和usigned long long版本,只需要在函数名后面加上I或I就可以了,比如__builtin_clzll
```

```C++
//reverse()对stl做翻转
//reserve()提前分配内存空间
//isdigit(c)检查字符c是否是数字
//isspace(c)检查字符c是否是空格
//to_string()数值转为string
//stoi() string转为int  stoll()转成long long类型
//static_cast<double>
//next_permutation用于得到下一个排列，主要是暴力情况
//iota 递增序列填充
//nth_element(b.begin(), b.begin() + k, b.end()) b.beign()+k为第k+1个小的元素，小于b.begin()+k的fan
```

~~~c++
const double pi=atan(1)*4;

cout<<fixed<<setprecision(20);

//将浮点数p保留两位小数存入str
stringstream ss;
ss << std::setiosflags(std::ios::fixed) << std::setprecision(2) << p;
string str = ss.str();
~~~

~~~C++
//decltype的作用是获取变量的类型

//tuple是一个固定大小的不同类型值的集合
//make_tuple用于构造一个tuple、tuple_size用于获取tuple的参数个数、get获取tuple指定索引的值
tuple<TreeNode*, int, int> t;
TreeNode* r=get<0>(t);
int r=get<1>(t),c=get<2>(t);

~~~

~~~C++
string s;
cin>>s;//不会读入一行中的空格
getline(cin,s);//会读入一行中的空格
~~~

~~~C++
///2是向0取整，正数向下负数向上，>>是向下取整 
~~~
