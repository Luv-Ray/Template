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
|stack|?不支持|
|queue|不支持|
|priority_queue|不支持|

运算符： ++ , --?, += , -= , iter' = (iter + n) , n = iter - iter'

list 仅有 ++，-- 操作，可以用`advance(iter,n)`向前移动n位

### set
自动对key排序，multiset key可重复（数据只有key）

核心：查找关键字是否存在

插入：`myset.insert(n)`? 返回pair, first为插入元素位置的迭代器，second值为是否插入成功

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

 ?
N,M可以动态指定，用 \* 代替M或者N，然后在参数列表里加上一个数字参数。

例子：`printf("%-*.*s", 5,2,"123");`

### cout

库?`<iomanip>`?包含了对输入输出的控制。

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
# 字符串
## 字符串有关函数
### std::string类
#### 截取：

`s.substr(pos, size);` pos为下标，size为截取长度

#### 反转：

`reverse(s.begin(), s.end());` 原地反转

`s1.assign(s.rbegin(), s.rend());` 反转并赋给s1

#### 更改：

`s.assign(str);`

`s.assign(str, pos, size);`

`s.assign(str, pos,string::npos);` 把字符串str从pos开始到结尾赋给s

`s.assign("nico", 5);` 把’n’ ‘I’ ‘c’ ‘o’ ‘\0’赋给字符串

`s.assign(n, 'x');` 把五个x赋给字符串

#### 删除：

`s.erase(pos);` 从索引pos开始往后全删除

`s.erase(pos, n);` 从索引pos开始往后删n个

`iterator erase(iterator it);` 删除it指向的字符，返回删除后迭代器的位置  

`iterator erase(iterator first, iterator last);` 删除[first，last）之间的所有字符

`s.erase(std::remove(s.begin(), s.end(), 'a'), s.end());` 删除所有特定字符

`s.erase(std::unique(s.begin(), s.end()) ,s.end());`
删除所有重复字符，要求s有序

#### 查找：

`s.find('c');` 从0开始查找字符c在当前字符串的位置

`s.find(str, pos);` 从pos开始查找str在当前串中的位置

`s.find(str, pos, n);` 从pos开始查找str中前n个字符在当前串中的位置

#### 大小写转换：

`transform(s.begin(), s.end(), s.begin(), ::toupper);`

`transform(s.begin(), s.end(), s.begin(), ::tolower);`

### 数字/字符串转换
#### 任意类型(整型、长整型、浮点型等)的数字转换为字符串

`itoa()`：将整型值转换为字符串。
`itoa(int n, char* s, int r)  //将10进制n转换为r进制并赋给s`

`ltoa()`：将长整型值转换为字符串。

`ultoa()`：将无符号长整型值转换为字符串。

`gcvt()`：将浮点型数转换为字符串，取四舍五入。

`ecvt()`：将双精度浮点型值转换为字符串，转换结果中不包含十进制小数点。

`fcvt()`：指定位数为转换精度，其余同ecvt()。

#### 将字符串转换为任意类型(整型、长整型、浮点型等)

参数：`const char *nptr, char **endptr, int base`

如：`str.c_str(), NULL, 10`

`atof()`：将字符串转换为双精度浮点型值。

`atoi()`：将字符串转换为整型值。

`atol()`：将字符串转换为长整型值。

`strtod()`：将字符串转换为双精度浮点型值，并报告不能被转换的所有剩余数字。

`strtol()`：将字符串转换为长整值，并报告不能被转换的所有剩余数字。

`strtoul()`：将字符串转换为无符号长整型值，并报告不能被转换的所有剩余数字。

## KMP
### next数组
```
char m[N];
int nex[N];
void get_nex(int l)
{
    int i = 0, j = -1;
    nex[i] = j;
    while (i < l)
    {
        while (j != -1 && m[i] != m[j])
            j = nex[j];
        nex[++i] = ++j;
    }
}
```
```
char m[N];
int nex[N];
void quick_get_nex(int l) // 优化版next数组，抄的模板，不太理解
{
    int i = 1, j = 0; // 从1开始
    nex[1] = 0;
    while (i < l)
    {
        if (j == 0 || m[i - 1] == m[j - 1]) //-1是为了和string 从零开始相对应
        {
            i++, j++;
            if (m[i - 1] != m[j - 1]) // 相同序列的第一位不同位不进行回溯，（已经不同了，回溯之后的位置的元素与它不同）
            {
                nex[i] = j;
            }
            else
                nex[i] = nex[j]; // 跳过一次无意义的比较
        }
        else
            j = nex[j];
    }
}
```
### 主串与模式串的匹配
```
char m[N], s[N];
int nex[N];
int ls = strlen(s), lm = strlen(m);
get_nex(lm, m, nex);
int cnt = 0;
int i = 0, j = 0;
while (i < ls) {
    while (j != -1 && s[i] != m[j])
        j = nex[j];
    j++;
    i++;
    if (j == lm)
        cnt++;
}
```
### next数组的周期性
1. 如果 i \% ( i - next[ i ] ) == 0, 则字符串 1 ~ i - next[ i ]为其最小的循环节。并且此字符串的循环节个数最多为 i / (i - next[ i ]);
   
2. 如果 i - next[ ? ]能够得到 i % ( i - next[ ? ] ) == 0， 则 i - next[ ? ]也为其循环节，但不是最小的循环节。此循环节的个数为 i / ( i - next[ ? ] );
   
3. 如果 i % ( i - next[ i ] ) != 0, 则此字符串不存在能够组成完整字符串的循环节。

4. next[ i ] 存在,循环节未必存在: 如 abcabcab 中, next[ 8 ] = 5,?但是对于 i 而言不存在循环节。但是 i - next[ i ] 必为重复的，只是最后的一段无法完全满足条件。8 - 5 == 3, 所以为 abc,abc 不断循环。
## Manacher
```
std::string s, str;
int p[N];
void add() { // 保证字符串长度为奇数
    str = "@#";
    int len = s.size();
    for (int i = 0; i < len; i++) {
        str += s[i];
        str += '#';
    }
    str += '$';
}
int mana() {
    int max = 0;
    int len = str.size() - 1;
    int r = 0, pos = 0;
    for (int i = 1; i < len; i++) {
        p[i] = r > i ? std::min(r - i, p[(pos << 1) - i]) : 1;
        while (str[i + p[i]] == str[i - p[i]]) p[i]++; // 中心拓展
        if (i + p[i] > r) { // 更新右范围
            pos = i;
            r = i + p[i];
        }
    }
    for (int i = 1; i < len; i++) {
        if (max < p[i]) max = p[i];
        p[i] = 0;
    }
    return max - 1;
}
```
## 字符串哈希
$P$ 一般取 $13331$ 或 $23333$，$MOD$ 为 $1e9+7$ 或 $1e9+9$
### 自然溢出
```
typedef unsigned long long ULL;
char s[N];
ULL hash[N], p[N]; // MOD为ULL上界，容易哈希冲突，不建议
const ULL P = 13331;
void Hash(int n) {
    p[0] = hash[0] = 1;
    for (int i = 1; i <= n; i++) {
        p[i] = p[i - 1] * P;
        hash[i] = hash[i - 1] * P + s[i];
    }
}
ULL query(int l, int r) {
    return hash[r] - hash[l - 1] * p[r - l + 1];
}
```
### 双哈希
```
char s[N];
LL hash1[N], hash2[N];
LL p1[N], p2[N];
const LL M1 = 1e9 + 7, M2 = 1e9 + 9;
const LL P1 = 13331, P2 = 23333;
void Hash(int n, LL *p, LL *hash, const LL P, const LL M) {
    p[0] = hash[0] = 1;
    for (int i = 1; i <= n; i++) {
        p[i] = p[i - 1] * P % M;
        hash[i] = (hash[i - 1] * P + s[i]) % M;
    }
}
LL query(int l, int r, LL *p, LL *hash, const LL M) {
    return ((hash[r] - hash[l - 1] * p[r - l + 1]) % M + M) % M;
}
```
## AC自动机
```
char s[N];
char str[N];
struct node {
    int son[26];
    int cnt;
    int fail;
} nod[N];
int cnt;
void insert(int len) { // 建Trie树
    int pos = 0;
    for (int k = 0; k < len; k++) {
        int t = s[k] - 'a';
        if (nod[pos].son[t] == 0) nod[pos].son[t] = ++cnt;
        pos = nod[pos].son[t];
    }
    nod[pos].cnt++;
}
void build() { // 构建fail指针
    std::queue<int> q;
    for (int i = 0; i < 26; i++) // 第一层节点入队
        if (nod[0].son[i]) q.push(nod[0].son[i]);
    while (!q.empty()) {
        int t = q.front(); q.pop(); // 当前节点
        int fail_t = nod[t].fail; // 当前节点失配指向的节点
        for (int i = 0; i < 26; i++) {
            int pos = nod[t].son[i]; // 当前节点的子节点
            int fail_pos = nod[fail_t].son[i]; // 失配节点的同子节点
            if (pos) {
                nod[pos].fail = fail_pos; // 有字节的就更新字节点的fail
                q.push(pos);
            }
            else {
                nod[t].son[i] = fail_pos; // 没子节点就填充字节点
            }
        }
    }
}
int match(int len) {
    int res = 0;
    for (int i = 0, j = 0; i < len; i++) {
        int t = str[i] - 'a';
        j = nod[j].son[t];
        int pos = j;
        while (pos) { // 对每个节点的失配指针判断
            res += nod[pos].cnt;
            if (nod[pos].cnt) nod[pos].cnt = 0;
            pos = nod[pos].fail;
        }
    }
    return res;
}
```
### AC自动机上的动态规划
Trie图上每个点都是一个状态，在AC自动机的状态相互转化，可以实现动态规划

1. 求模式串在原串中出现的次数：（所有模式串不同）
   
    构建Trie树时，在每个串结尾节点进行标记，在AC自动机上匹配时，每遇到一次结尾节点即表示成功匹配，ans++

2. 求模式串在原串中出现次数，若模式串B是模式串A的子串，则只记录A：
   
    构建Trie树时，在每个串结尾节点进行标记，在AC自动机上匹配时，对经过的子串模式串消除标记，其余与之前相同

3. 求原串中不包含任何模式串的串的种类数：
   
    对所有模式串构建AC自动机，模式串的终止的都是非法的，不可经过dp[i][j]表示长度为i，状态为j的字符串的种类数，枚举所有字符进行状态匹配，答案即为sum(dp[m][i])若m较小，n较大，可以考虑用矩阵乘法加速dp

4. 求一个长度最短的串使得它包含所有模式串：
   
    对所有模式串建立AC自动机，若n很小，可用状态压缩dp
    二进制状态 $j\&2^k$ 表示从根节点到该结点的路径上有第k个模式串 $dp[i][j]$ 表示状态 i ，二进制状态j的最短长度，初始化 $dp[i][j] = 0;$
    在Trie上求 $(0,0)$ 到$(i,2^{n-1})$点的最短路，答案即是 $dp[i][2^{n-1}]$
# 数据结构
## 并查集
```
int fa[N];
int find(int x) {
	return x == fa[x] ? x : fa[x] = find(fa[x]);
}
void merge(int u, int v) {
	int uu = find(u), vv = find(v);
	if (uu != vv) fa[uu] = vv;
}
```
## 树上问题
### 直径
两次dfs求直径
```
std::vector<int> e[N];
int max, ith; // 最大长度和端点
int deep[N], fat[N];
void dfs(int pos, int fa, int dep) {
    fat[pos] = fa;
    deep[pos] = dep;
    if (dep > max) {
        max = dep;
        ith = pos;
    }
    for (auto i : e[pos]) {
        if (i != fa) dfs(i, pos, dep + 1);
    }
}
```
### 重心
所有子树的**最大节点数** **最小**的节点称为树的重心
```
std::vector<int> e[N];
int min = INF, ith;
int dfs(int pos, int fa) {
    int dep = 1;
    int res = 0; // 本节点所有子树的最大节点数
    for (auto i : e[pos]) {
        if (i == fa) continue;
        int t = dfs(i, pos); // 当前子树节点数
        res = std::max(res, t);
        dep += t;
    }
    res = std::max(res, n - dep);
    if (res < min) {
        min = res;
        ith = pos;
    }
    return dep;
}
```
### LCA
```
for (int i = 1; i <= n; i++) // 预处理二进制位数，等于log2(i) + 1！
        lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
```
```
std::vector<int> e[N];
int lg[N];
int fa[N][M], dep[N];
void dfs(int pos, int fath) { // 记录深度和祖先
    fa[pos][0] = fath;
    dep[pos] = dep[fath] + 1;
    for (int i = 1; i <= lg[dep[pos]]; i++)
        fa[pos][i] = fa[fa[pos][i - 1]][i - 1]; // 2 = 1 + 1
    for (auto i : e[pos])
        if (i != fath) dfs(i, pos);
}
int lca(int a, int b) {
    if (dep[a] < dep[b]) std::swap(a, b);
    while (dep[a] > dep[b]) 
        a = fa[a][lg[dep[a] - dep[b]] - 1]; // 注意减一！
    if (a == b) return a;
    for (int i = lg[dep[a]] - 1; i >= 0; i--) // 从大到小减
        if (fa[a][i] != fa[b][i])
            a = fa[a][i], b = fa[b][i];
    return fa[a][0];
}
```
### 差分
利用递归回收标记，因为从下到上路径唯一

对于每条链：起点++（包含递增），终点--（不包含递增）
```
int p[N];
int dfs_sum(int pos, int fath) {
    int sum = p[pos];
    for (auto i : e[pos]) {
        if (i == fath) continue;
        sum += dfs_sum(i, pos);
    }
    return sum;
}
```
#### 点差分
拆成三条链看待：
1. `a -> lca`
2. `b -> lca`
3. `lca -> fa[lca]`
```
int res = lca(a, b);
int fath = fa[res][0];
p[res]--;
p[fath]--;
p[a]++;
p[b]++;
```
#### 边差分
拆成两条链看待：
1. `a -> lca`
2. `b -> lca`
```
int res = lca(a, b);
p[a]++;
p[b]++;
p[res] -= 2;
```
## ST表
+ 1e4 14
+ 1e5 17
+ 1e6 20
+ 1e7 24
+ 1e8 27
+ 1e9 30
```
int max[N][M];
int query(int l, int r) {
    int k = log2(r - l + 1);
    return std::max(max[l][k], max[r - (1 << k) + 1][k]);
}
```
```
for (int i = 1; i <= n; i++) cin >> max[i][0];
for (int l = 1; l <= M; l++) {
    for (int i = 1; i + (1 << l) - 1 <= n; i++) {
        max[i][l] = std::max(max[i][l - 1], max[i + (1 << (l - 1))][l - 1]);
    }
}
```
## 树状数组
`inline int lowbit(int x) { return x & -x; }`
### 单点修改，区间查询
```
void update(int x, int k, int n) {
	for (int i = x; i <= n; i += lowbit(i))
		t[i] += k;
}
int getsum(x) {
	int sum = 0;
	for(int i = x; i; i -= lowbit(i)) {
		sum += t[i];
	}
	return sum;
}
```
### 区间修改，单点查询
构造出原数组的差分数组，然后用树状数组维护（转换为单点修改，区间查询）

利用差分数组的性质，其前缀和即为该元素本身
```
update(l, x, n);
update(r + 1, -x, n);
```
### 区间修改，区间查询
构造差分数组 $sum1 [ n ], sum2 = sum1 [ i ] * i;$ ( i?从1开始计数 )
```
void update(ll p, ll x) {
    for(int i = p; i <= n; i += lowbit(i))
        sum1[i] += x, sum2[i] += x * p;
}
void range_update(ll l, ll r, ll x) {
    update(l, x), update(r + 1, -x);
}
ll getsum(ll p) {
    ll res = 0;
    for(int i = p; i; i -= lowbit(i))
        res += (p + 1) * sum1[i] - sum2[i]; // 重点！
    return res;
}
ll range_getsum(ll l, ll r) {
    return getsum(r) - getsum(l - 1);
}
```
## 线段树
```
struct tree {
    int l, r, s, lz;
} t[N << 2]; // 开四倍
int a[N];
inline int gls(int i) { return i << 1; } // 左子
inline int grs(int i) { return i << 1 | 1; } // 右子
void build(int i, int l, int r) { // 建树
    t[i].l = l, t[i].r = r;
    if (l == r) {
        t[i].s = a[l];
        return;
    } 
    int mid = l + r >> 1;
    int ls = gls(i), rs = grs(i);
    build(ls, l, mid);
    build(rs, mid + 1, r);
    t[i].s = t[ls].s + t[rs].s;
}
void push_down(int i) { // 懒标记
    int z = t[i].lz;
    if (z) {
        t[i].lz = 0;
        int ls = gls(i), rs = grs(i);
        t[ls].lz += z;
        t[rs].lz += z;
        int mid = t[i].l + t[i].r >> 1;
        t[ls].s += z * (mid - t[ls].l + 1);
        t[rs].s += z * (t[rs].r - mid);
    }
}
void add(int i, int x, int y, int k) { // 区间修改
    if (x <= t[i].l && t[i].r <= y) {
        t[i].s += k * (t[i].r - t[i].l + 1);
        t[i].lz += k;
        return;
    }
    push_down(i);
    int ls = gls(i), rs = grs(i);
    if (t[ls].r >= x) add(ls, x, y, k);
    if (t[rs].l <= y) add(rs, x, y, k);
    t[i].s = t[ls].s + t[rs].s;
}
int search(int i, int x, int y) { // 区间查询
    if (x <= t[i].l && t[i].r <= y) return t[i].s;
    push_down(i);
    int ls = gls(i), rs = grs(i);
    int res = 0;
    if (t[ls].r >= x) res += search(ls, x, y);
    if (t[rs].l <= y) res += search(rs, x, y);
    return res;
}
```
# 动态规划
## 思路
对一个dp状态的定义，**假设已经**得到了最优子结构，去考虑能否完整得进行状态转移

将初始条件和转移过程分开讨论，防止全局考虑时过于复杂导致思路受阻
## 背包
### 内层循环的下限优化（适用V较大的情况）

```
for (int i = 1; i <= n; i++)
	cin >> c[i] >> w[i], s[i] = s[i - 1] + c[i];
for (int i = 1; i <= n; i++) {
	int bound = max(c[i], V - (s[n] - s[i]));
	for (int j = V; j >= bound; j--)
		f[j] = max(f[j], f[j - c[i]] + w[i]);
}
```
### 有依赖的背包模板
```
int dp[N], last[N];
void solve()
{
    int n = read(), V = read(); // n主件个数，V总钱数
    for (int i = 1; i <= n; i++)
    {
        memcpy(last, dp, (V + 1) << 2); // 需要继承上一组物品的最优情况，可避免使用二维数组
        int v = read(), m = read();    // v主件费用，m附件个数
        for (int k = 1; k <= m; k++)
        {
            int c = read(), w = read(); // c附件费用、w附件价值
            for (int j = V - v; j >= c; j--)
                last[j] = max(last[j], last[j - c] + w);
        }
        for (int j = v; j <= V; j++)
            dp[j] = max(dp[j], last[j - v]); // f数组为加上主件的费用后的最大价值
    }
    printf("%d\n", dp[V]);
}
```
## 状压dp
### tricks
#### 遍历一个集合的子集
对于一个集合 $S$ ：`for (int i = S; i; i = (i - 1) & S)`

则状态转移：`dp[S] = std::max(dp[S], dp[S ^ i] + s[i]);`
## 数位dp
### 数位dp基本思路（摘自洛谷）
```
dfs(数的最后若干位, 各种限制条件, 当前第几位)
	if 最后一位
    	return 各种限制条件下的返回值
    局部变量 ct = 当前位的数字
    局部变量 sum = 0;
    for i = 0 to ct - 1
    	sum += 当前位取i时一定无无限制的合法状态数
        sum += 当前位取i时满足当前限制的合法状态数
    根据ct更新限制条件 不再满足则return sum
    return sum + dfs(当前位后的若干位,更新后的限制条件,下一位)

slv(当前数)
	if(只有一位) return 对应的贡献
    局部变量 ct;
    for ct = 可能最高位 to 1
    	if 当前位有数字 break
    局部变量 nw = 当前位数字
    局部变量 sum = 0
    for i = 1 to nw - 1
    	sum+=当前位取i后合法情况任意取的贡献
    for i = 1 to ct - 1
    	for j = 1 to 9
        	sum += 第i位取j后合法情况任意取的贡献
    sum += dfs(去掉第一位后的若干位,限制条件,第二位)
    return sum

main
	预处理当前位取i的各种条件各种限制的贡献
    读入 L R
    --L
    输出 slv(R) - slv(L)
    return 0
```
# 图论
## 存/建图
### 链式前向星 $O(m)$
```
struct edge {
	int next, to, weight;
} e[MAX];
int head[MAX], dis[MAX], vis[MAX], cnt;
void add(int u, int v, int w) {
	e[++cnt].next = head[u];
	e[cnt].to = v;
	e[cnt].weight = w;
	head[u] = cnt;
}
```
遍历循环条件（按节点遍历以该点为起点的所有边）
`for (int i = head[x]; i; i = e[i].next)`

距离交换条件
`if (dis[e[i].to] > dis[x] + e[i].weight)`
## 最小生成树
### Prim 稠密图 $O(n^2)$
```
int dis[N], vis[N], edge[N][N];
int Prim(int pos) {
	memset(dis, 0x3f, sizeof dis);
	dis[pos] = 0;
	for (int i = 1; i <= n; i ++) {
		int cur = 0;
		for (int j = 1; j <= n; j ++) {
			if (!vis[j] && (!cur || dis[j] < dis[cur])) {
				cur = j; // 寻找一个权值最小的点, 记录下标
			}
		}
		if (dis[cur] >= INF) return INF;
		sum += dis[cur];
		vis[cur] = 1;
		for (int k = 1; k <= n; k++) {
			if (!vis[k]) dis[k] = min(dis[k], edge[cur][k]);
		}
	}
	return sum;
}
```
### Prim 堆优化 $O(nlogn)$
```
typedef std::pair<int, int> PI;
int dis[N], vis[N], edge[N][N];
int Prim(int pos) {
	memset(dis, 0x3f, sizeof dis);
	dis[pos] = 0;
	priority_queue<PI, std::vector<PI>, std::greater<PI> > q;
	q.push({0, pos});
	while (!q.empty()) {
		int w = q.top().first, u = q.top().second;
		q.pop();
		if (vis[u]) continue;
		vis[u] = 1;
		sum += w;
		for (int k = 1; k <= n; k++) {
			if (edge[u][k] < dis[k]) {
				dis[k] = edge[u][k];
				q.push({dis[k], k});
			}
		}
	}
	return sum;
}
```
### Kruskal 稀疏图 $O(mlogm)$
```
struct edge {
	int u, v, w;
} r[N];
bool cmp(const edge &a, const edge &b) {
	return a.w > b.w;
}
int fa[N]; // 利用并查集
int find(int x) {
	return x == fa[x] ? x : fa[x] = find(fa[x]);
}
int Kruskal(int m) {
	std::sort(r, r + m, cmp);
	int sum = 0;
	for (int i = 0; i < m; i++) {
		int uu = find(r[i].u), vv = find(r[i].v);
		if (uu != vv) {
			fa[uu] = vv;
			sum += r[i].w;
		}
	}
	return sum;
}
```
## 最短路
### Dijkstra $O(n^2)$
```
for (int j = 1; j <= n; j ++ )
    if (!vis[j] && (t == -1 || dis[t] > dis[j])) // 遍历找到当前最短节点t
       t = j;
vis[t]=1;
for (int j = 1; j <= n; j ++ )
    dis[j] = min(dis[j], dis[t] + e[t][j]); // 根据t更新dis
```
### Dijkstra 堆优化 $O(nlogn)$
```
typedef std::pair<int,int> PI;
priority_queue<PI, std::vector<PI>, std::greater<PI> > q;
while (!q.empty()) {
	int x = q.top().second;
	q.pop();
	if (vis[x]) continue;
	vis[x] = 1;
	for (int i = head[x]; i; i = e[i].next) {
		if (dis[e[i].to] > dis[x] + e[i].weight) {
			dis[e[i].to] = dis[x] + e[i].weight;
			q.push({dis[e[i].to], e[i].to});
		}
	}
}
```
### SPFA $O(mn)$
```
int vis[N], dis[N], cnt[N];
std::vector<std::pair<int, int> > e[N];
bool SPFA() {
	std::queue<int> q;
	q.push(1); // 此时1为源点，若无源点就将全部点放入，不初始化dis数组
	dis[1] = 0;
	while (!q.empty()) {
		int x = q.front();
		q.pop();
		vis[x] = 0;
		for (int i = 0; i < e[x].size(); i++) {
			int vv = e[x][i].first, ww = e[x][i].second;
			if (dis[vv] > dis[x] + ww) {
				dis[vv] = dis[x] + ww;
				cnt[vv] = cnt[x] + 1;
				if (cnt[vv] >= n) {
					return 1; // 负环返回1
				}
				if (!vis[vv]) {
					q.push(vv);
					vis[vv] = 1;
				}
			}
		}
	}
	return 0;
}
```
### Floyd $O(n^3)$
```
void Floyd() {
	for (int k = 1; k <= n; k ++ ) // 三重循环更新各个点之间距离，k为中间媒介
		for (int i = 1; i <= n; i ++ )
			for (int j = 1; j <= n; j ++ )
				dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
}
```
# 数学
## 线性代数
### 矩阵
```
struct Matrix {
    int a[N][N];
    inline void init() { // 零矩阵
		memset(a, 0, sizeof a);
	}
	inline void unit() { // 单位矩阵
		init();
		for (int i = 0; i < N; i++) a[i][i] = 1;
	}
};
```
#### 矩阵运算
```
//重载矩阵加法
Matrix operator+(const Matrix &a, const Matrix &b) {
	Matrix c;
	c.init();
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			c.m[i][j] = (a.m[i][j] + b.m[i][j]) % MOD;
		}
	}
	return c;
}
//重载矩阵减法
Matrix operator-(const Matrix &a, const Matrix &b) {
	Matrix c;
	c.init();
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			c.m[i][j] = ((a.m[i][j] - b.m[i][j]) % MOD + MOD) % MOD;
		}
	}
	return c;
}
//重载矩阵乘法
Matrix operator*(const Matrix &a, const Matrix &b) {
	Matrix c;
	c.init();
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			for (int k = 0; k < N; k++) {
				c.m[i][j] += a.m[i][k] * b.m[k][j] % MOD;
				c.m[i][j] %= MOD;
			}
		}
	}
	return c;
}
//重载矩阵比较符
bool operator==(const Matrix &a, const Matrix &b) {
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			if (a.m[i][j] != b.m[i][j]) {
				return 0;
			}
		}
	}
	return 1;
}
bool operator!=(const Matrix &a, const Matrix &b) {
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			if (a.m[i][j] != b.m[i][j]) {
				return 1;
			}
		}
	}
	return 0;
}
```
#### 矩阵快速幂
```
Matrix Mqpow(Matrix a, LL b) {
	Matrix res;
	res.unit();
	while (b) {
		if (b & 1) res = res * a;
		a = a * a;
		b >>= 1;
	}
	return res;
}
```
### 高斯消元
```
Matrix m;
int r, c;
cin >> r >> c;
for (int i = 0; i < r; i++) {
	for (int j = 0; j < c; j++) {
		cin >> m.a[i][j];
	}
}

for (int i = 0; i < r; i++) {
	int max = i;
	for (int j = i + 1; j < c; j++) {
		if (fabs(m.a[j][i] > fabs(m.a[max][i])))
			max = j;
	}
	if (fabs(m.a[max][i]) < EPS) {
		cout << "No Solution" << endl;
		return;
	}
	if (max != i) std::swap(m.a[max], m.a[i]);

	double div = m.a[i][i];
	for (int j = i; j < c; j++) {
		m.a[i][j] /= div;
	}
	for (int j = i + 1; j < r; j++) {
		div = m.a[j][i];
		for (int k = i; k < c; k++) {
			m.a[j][k] -= m.a[i][k] * div;
		}
	}
}

std::vector<double> ans(r);
ans[r - 1] = m.a[r - 1][c - 1];
for (int i = r - 2; i >= 0; i--) {
	ans[i] = m.a[i][c - 1];
	for (int j = i + 1; j < r; j++) {
		ans[i] -= m.a[i][j] * ans[j];
	}
}
```
## 组合数学
### 求组合数
```
int fac[N], inv[N]
void init(int n) { // 预处理逆元
	fac[0] = inv[0] = 1;
	for(int i = 1; i <= n; i++) {
		fac[i] = fac[i - 1] * i % MOD;
	}
	inv[n] = qpow(fac[n], MOD - 2);
	for (int i = n - 1; i; i--) {
		inv[i] = inv[i + 1] * (i + 1) % MOD;
	}
}
int C(int n, int m) {
	return fac[n] * inv[m] % MOD * inv[n - m] % MOD;
}
```
# 数论
## 素数
### 知识点
#### 欧拉函数特性
1. 若 $i$ 为质数, $o[i] = i - 1;$

2. 若 $i$ 为质数, $n \% i == 0, o[i * n] = o[n] * i;$

3. 若 $n$ 和 $i$ 互质, $o[i * n] = o[n] * o[i];$

### 线性筛
```
int st[N], primes[N], cnt;
void get_primes(int n) {
	for (int i = 2; i <= n; i++) {
		if (!st[i])
			primes[cnt++] = i;
		for (int j = 0; primes[j] <= n / i; j++) {
			st[i * primes[j]] = 1;
			if (i % primes[j] == 0)
				break;
		}
	}
}
```
### 线性筛同时求欧拉函数
```
int st[N], primes[N], o[N], cnt;
void euler(int n) {
	for (int i = 2; i <= n; i++) {
		if (!st[i]) {
			primes[cnt++] = i;
			o[i] = i - 1;
		}
		for (int j = 0; primes[j] <= n / i; j++) {
			st[i * primes[j]] = 1;
			if (i % primes[j] == 0) {
				o[i * primes[j]] = o[i] * primes[j]; // 特性2
				break;
			}
			o[i * primes[j]] = o[i] * (primes[j] - 1); // 特性3
		}
	}
}
```
### 求单个欧拉函数
```
int euler(int n) {
	int ans = n;
	for (int i = 2; i <= n / i; i++) {
		if (n % i == 0) {
			ans = ans / i * (i - 1);
			while (n % i == 0)
				n /= i;
		}
	}
	if (n > 1)
		ans = ans / n * (n - 1);
	return ans;
}
```
### 欧拉函数打表（效率同线性筛版）
```
int E[N];
void n_euler_q(int n) // 欧拉函数打表
{
	for (int i = 2; i <= n; i++)
	{
		if (!E[i])
			for (int j = i; j <= n; j += i)
			{
				if (!E[j])
					E[j] = j;
				E[j] = E[j] / i * (i - 1);
			}
	}
}
```
### Miller_Rabin素数测试
```
bool MRtest(LL n) {
    if (n < 3 || n % 2 == 0)
        return n == 2; // 特判
    LL u = n - 1, t = 0;
    while (u % 2 == 0)
        u /= 2, ++t;
    const LL ud[] = {2, 325, 9375, 28178, 450775, 9780504, 1795265022};
    for (LL a : ud) {
        LL v = qpow(a, u, n);
        if (v == 1 || v == n - 1 || v == 0)
            continue;
        for (int j = 1; j <= t; j++) {
            v = qmul(v, v, n); // ！要用防long long溢出的快速幂
            if (v == n - 1 && j != t) {
                v = 1;
                break;
            } // 出现一个n-1，后面都是1，直接跳出
            if (v == 1)
                return 0; // 这里代表前面没有出现n-1这个解，二次检验失败
        }
        if (v != 1)
            return 0; // Fermat检验
    }
    return 1;
}
```
## 同余
### 知识点
#### 费马小定理
如果 $p$ 是一个质数，而整数 $a$ 不是 $p$ 的倍数, 则有 $a ^{ p - 1 } \equiv 1 \pmod p$
### 扩展欧几里得
```
int exgcd(int a, int b, int &x, int &y) // 扩展欧几里得
{
	if (!b) {
		x = 1, y = 0;
		return a;
	}
	int res = exgcd(b, a % b, x, y);
	int xx = x;
	x = y, y = xx - a / b * y; // x`= y, y` = x - a / b * y
	return res;
}
```
### 乘法逆元
#### 使用范围：
1. $n\perp p$ 是逆元的存在条件，否则无解
2. $p$ 不必为质数：扩展欧几里得
3. 要求 $p$ 为质数：费马小定理 `qpow(n, m - 2, m)`，运行慢，基本不用
```
int inv(int n, int m) // n的乘法逆元
{
	int x, y;
	exgcd(n, m, x, y);
	return (x % m + m) % m; // 将范围约束在[0,m-1]以内
}
```
#### 线性求逆元
```
int inv[N];
void n_inv(int n, int m) // [1,n] 线性求逆元，使用范围同费马小定理
{
	inv[1] = 1;
	for (int i = 2; i <= n; ++i)
	{
		inv[i] = (-1 * m / i + m) * inv[m % i] % m;
	}
}
```
### 同余方程组
```
struct func {
    int m, r; // 对m取模余r
} f[N];
```
#### 中国剩余定理
```
int crt(int n) {
    int M = 1;
    for (int i = 0; i < n; i++)
		M *= f[i].m; // 要求模数互质，M为最小公倍数
    int res = 0;
    for (int i = 0; i < n; i++) {
        int t = M / f[i].m;
        res = (res + f[i].r * t * inv(t, f[i].m)) % M;
    }
    return res;
}
```
#### 扩展中国剩余定理
```
int excrt(int n) {
    int M = f[0].m;
    int A = f[0].r;
    for (int i = 1; i < n; ++i) {
        int x = 0, y = 0;
        int ch = f[i].r - A;
        int r = exgcd(M, f[i].m, x, y);
        if (ch % r) return -1;
        A = ch / r * x * M + A; //乘法容易溢出，可考虑__int128或者快速乘
        M = M / r * f[i].m;
        A = (A % M + M) % M;
    }
    return A;
}
```
## 散装函数
### 快速幂
```
int qpow(int a, int b, int m) {
	int res = 1;
	while (b) {
		if (b & 1)
			res = (res % m * a) % m;
		a = (a % m) * a % m;
		b >>= 1;
	}
	return res;
}
```
### 快速乘
```
LL qmul(LL a, LL b, LL mod) {
    LL c = (long double)a / mod * b;
    LL res = (ULL)a * b - (ULL)c * mod; // 用溢出解决溢出
    return (res + mod) % mod;
}
```
### 最大公因数 $O(log(a + b))$
```
int gcd(int a, int b) {
	return b ? gcd(b, a % b) : a;
}
```
### 最小公倍数
```
int lcm(int a, int b) {
	return a / gcd(a, b) * b;
}
```
### 唯一分解定理
```
int num[N], prime[N]; // primes数组为筛好的质数
void divide(int x) {
	int cnt = 0;
	for (int i = 0; primes[i] <= x / primes[i]; i++) {
		if (x % p[i] == 0) {
			prime[++cnt] = primes[i];
			while (x % primes[i] == 0) {
				x /= primes[i];
				num[cnt]++;
			}
		}
	}
	if (x > 1) prime[++cnt] = x, num[cnt] = 1;
}
```
推论：对于 $N = p_{1}^{c_{1}}p_2^{c_{2}}...p_{k}^{c_{k}}$ 
1. $N$ 的某个正约数可以表示成：$d = p_{1}^{a_{1}}p_2^{a_{2}}...p_{k}^{a_{3}}, \quad(0 \leq a_{i} \leq c_{i},i \in[1, k])$

2. $N$ 的正约数个数为：$(c_{1}+1) * (c_{2}+1) * ... * (c_{k}+1)$

3. $N$ 的所有正约数之和为： $(1+p_{1}+p_{1}^2+..+p_{1}^{c_{1}}) * (1+p_{2}+p_{2}^2+..+p_{2}^{c_{2}})*... * (1+p_{k}+p_{k}^2+..+p_{k}^{c_{k}})$（例如12的所有正约数为1，2，3，4，6，12，那么所有正约数之和为1 + 2 + 3 + 4 + 6 + 12 = 28。）
