# 语法细节
## STL
### 迭代器
声明：`std::vector<int>::(const_/reverse_)iterator iter;`
|容器|访问|
|---|---|
|vector|随机访问|
|deque|随机访问|
|list|双向|
|set / multiset|双向|
|map / multimap|双向|
|stack| 不支持|
|queue|不支持|
|priority_queue|不支持|

运算符： ++ , -- , += , -= , iter' = (iter + n) , n = iter - iter'

list 仅有 ++，-- 操作，可以用`advance(iter,n)`向前移动n位

### set
自动对key排序，multiset key可重复（数据只有key）

核心：查找关键字是否存在

插入：`myset.insert(n)`  返回pair, first为插入元素位置的迭代器，second值为是否插入成功

`lowe_bound()`大于等于某值第一个元素,`upper_bound()`大于某值第一个元素（二分查找）

find类函数找不到时返回`name.end()`
### map
自动对key排序，multimap key可重复( key - value一对一 )
```
map<int, string> m;
m.insert(pair<int, string>(000, "aaa")); // 第一种 插入pair
m.insert(map<int, string>::value_type(001, "aaa")); // 第二种 插入value_type数据
m[002] = "aaa"; // 第三种 用"array"方式插入
```
insert函数 (返回pair，first为map<>::iterator，second为bool) 不能覆盖旧值，数组方式可以

m[n]若不存在，直接按数组查找会生成元素，用`m.find()`确定是否存在
### pair(二元struct)
用`first`和`second`访问

定义时初始化：`pair<string,int> p("aaa", 999);`

用make_pair创建新pair：`p = make_pair("777", 777);`

用tie接收pair型返回值：`int n, m; std::tie(n, m) = (pair型返回值);`
### vector，list，deque
>需要高效的随即存取，而不在乎插入和删除的效率，使用vector
>
>需要大量的插入和删除，而不关心随即存取，则应使用list（没有提供[]操作符的重载）
>
>需要随即存取，而且关心两端数据的插入和删除，则应使用deque。
#### vector
`vector v1;` vector保存类型为T的对象。默认构造函数v1为空。

`vector v2(v1);` v2是v1的一个副本。

`vector v3(n, i);` v3包含n个值为i的元素。

`vector v4(n);` v4含有值初始化的元素的n个副本。
#### deque
deque提供了两级数组结构， 第一级完全类似于vector，代表实际容器；另一级维护容器的首位地址。

deque除了具有vector的所有功能外， 还支持高效的首/尾端插入/删除操作，在功能上合并了vector和list。
#### list
什么fw
### priority_queue
`priority_queue<int> q` 相当于 `priority_queue<int, vector<int>, less<int> > q;` 默认大顶堆

`priority_queue<int, vector<int>, greater<int> > q;` 小顶堆

## 输出
### 快读函数
```
inline int read() {
    int res = 0, f = 0;
    char c = getchar();
    while (!isdigit(c)) {
        if (c == '-') f = 1;
        c = getchar();
    }
    while (isdigit(c))
        res = (res << 3) + (res << 1) + c - 48, c = getchar();
    if (f) return -res;
    return res;
}
```
### printf
输出指定长度（N）的字符串, 超长时截断（限制大小M）, 不足时右对齐（空格填充）`printf("%N.Ms", str);`

改为左对齐`printf("%-N.Ms", str);`

  
N,M可以动态指定，用 \* 代替M或者N，然后在参数列表里加上一个数字参数。

例子：`printf("%-*.*s", 5,2,"123");`

### cout

库 `<iomanip>` 包含了对输入输出的控制。

`cout << fixed << setprecision(3) << i << endl; // 控制小数点后精度`

## 输入
(大约花费时间) `cin = getline(cin) > scanf > getchar > fgets`

用`scanf`读入`string`：`s.resize(n); scanf("%s", s);`

此时s大小必为n，不足会自动填充。
### 返回值
`fgets(char *str, MAX_SIZE, stdin);`会读取并保存换行符，结束时返回空指针( 0 )

`getline(cin, str);` `cin.getline(char *str, MAX_SIZE);`保留前置空格，不保留结束字符，遇到EOF返回0

`>>`操作重载函数遇到EOF返回0

`scanf`返回值为输入字符数，文件结尾返回EOF
## 各种函数
### 内建函数
```
__builtin_xxxll(n) // long long 版

__builtin_ffs(n) // 末尾1的位数 (ctz + 1)
__builtin_clz(n) // 前导0个数
__builtin_ctz(n) // 末尾0个数

__builtin_popcount(n) // 1的个数
__builtin_parity(n) // 1的个数的奇偶 (popcount % 2)
```
### memset初始化
```
memset(a,0x3F,sizeof(a)); //set int to 1061109567
memset(a,0x7F,sizeof(a)); //set int to 2139062143

memset(a,0xc0,sizeof(a)); //set int to -1061109568
memset(a,0x80,sizeof(a)); //set int to -2139062144

memset(a,0x7F,sizeof(a)); //set double to 1.38242e+306
memset(a,0xFE,sizeof(a)); //set double to -5.31401e+303
```

## 卡常细节
位操作具备更快的运算速度（编译器比你懂）

`std::vector<bool>`的效率会很慢，用`std::bitset`

每次调用函数时都会重新创建它的形参，并用传入的实参对形参进行初始化
（实参被值传递时相当于创建实参的拷贝），拷贝大的类类型对象或者容器对象比较低效
（比如vector，或者一个很长的string），如果函数无须改变形参的值，最好将其声明为常量引用。

`inline int cmp(string a,string b) {return a<b;}`

改为
`inline int cmp(const string &a,const string &b) {return a<b;}`



