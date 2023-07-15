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
```cpp
char m[N];
int nex[N];
void get_nex(int l) {
    int i = 0, j = -1;
    nex[i] = j;
    while (i < l) {
        while (j != -1 && m[i] != m[j]) {
            j = nex[j];
        }
        nex[++i] = ++j;
    }
}
```
### 主串与模式串的匹配
```cpp
char m[N], s[N];
int nex[N];
int ls = strlen(s), lm = strlen(m);
get_nex(lm, m, nex);
int cnt = 0;
int i = 0, j = 0;
while (i < ls) {
    while (j != -1 && s[i] != m[j]) {
        j = nex[j];
    }
    i++;
    j++;
    if (j == lm) {
        cnt++;
    }
}
```
### next数组的周期性
1. 如果 `i % (i - next[i]) == 0`, 则字符串 1 ~ i - `next[i]`为其最小的循环节。并且此字符串的循环节个数最多为 `i / (i - next[i])`;
   
2. 如果 `i - next[?]`能够得到 `i % (i - next[?]) == 0`， 则 i - `next[?]`也为其循环节，但不是最小的循环节。此循环节的个数为 `i / (i - next[?])`;
   
3. 如果 `i % (i - next[i]) != 0`, 则此字符串不存在能够组成完整字符串的循环节。

4. `next[i]` 存在,循环节未必存在: 如 abcabcab 中, next[ 8 ] = 5, 但是对于 i 而言不存在循环节。但是 i - `next[i]` 必为重复的，只是最后的一段无法完全满足条件。8 - 5 == 3, 所以为 abc,abc 不断循环。
## Manacher
```cpp
std::string s, str;
int p[N];
void init() {
    str += '$';
    str += '#';
    for (auto i : s) {
        str += i;
        str += '#';
    }
    str += '&';
}
int manacher() {
    int rmid = 0, rboun = 0;
	int len = str.size() - 2;
    for (int i = 1; i <= len; i++) {
        int j = 2 * rmid - i;
		p[i] = rboun > i ? std::min(rboun - i, p[j]) : 0;
        while (str[i - 1 - p[i]] == str[i + 1 + p[i]]) p[i]++;
        if (i + p[i] > rboun) {
			rmid = i;
			rboun = i + p[i];
		}
	}
    return 0;
}
```
## 字符串哈希
$P$ 一般取 $13331$ 或 $23333$，$MOD$ 为 $1e9+7$ 或 $1e9+9$
### 自然溢出
```cpp
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
```cpp
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
```cpp
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
