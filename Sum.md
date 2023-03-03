# �﷨ϸ��
## STL
### ������
������`std::vector<int>::(const_/reverse_)iterator iter;`
|����|����|
|---|---|
|vector|�������|
|deque|�������|
|list|˫��|
|set / multiset|˫��|
|map / multimap|˫��|
|stack|?��֧��|
|queue|��֧��|
|priority_queue|��֧��|

������� ++ , --?, += , -= , iter' = (iter + n) , n = iter - iter'

list ���� ++��-- ������������`advance(iter,n)`��ǰ�ƶ�nλ

### set
�Զ���key����multiset key���ظ�������ֻ��key��

���ģ����ҹؼ����Ƿ����

���룺`myset.insert(n)`? ����pair, firstΪ����Ԫ��λ�õĵ�������secondֵΪ�Ƿ����ɹ�

`lowe_bound()`���ڵ���ĳֵ��һ��Ԫ��,`upper_bound()`����ĳֵ��һ��Ԫ�أ����ֲ��ң�

find�ຯ���Ҳ���ʱ����`name.end()`
### map
�Զ���key����multimap key���ظ�( key - valueһ��һ )
```
map<int, string> m;
m.insert(pair<int, string>(000, "aaa")); // ��һ�� ����pair
m.insert(map<int, string>::value_type(001, "aaa")); // �ڶ��� ����value_type����
m[002] = "aaa"; // ������ ��"array"��ʽ����
```
insert���� (����pair��firstΪmap<>::iterator��secondΪbool) ���ܸ��Ǿ�ֵ�����鷽ʽ����

m[n]�������ڣ�ֱ�Ӱ�������һ�����Ԫ�أ���`m.find()`ȷ���Ƿ����
### pair(��Ԫstruct)
��`first`��`second`����

����ʱ��ʼ����`pair<string,int> p("aaa", 999);`

��make_pair������pair��`p = make_pair("777", 777);`

��tie����pair�ͷ���ֵ��`int n, m; std::tie(n, m) = (pair�ͷ���ֵ);`
### vector��list��deque
>��Ҫ��Ч���漴��ȡ�������ں������ɾ����Ч�ʣ�ʹ��vector
>
>��Ҫ�����Ĳ����ɾ�������������漴��ȡ����Ӧʹ��list��û���ṩ[]�����������أ�
>
>��Ҫ�漴��ȡ�����ҹ����������ݵĲ����ɾ������Ӧʹ��deque��
#### vector
`vector v1;` vector��������ΪT�Ķ���Ĭ�Ϲ��캯��v1Ϊ�ա�

`vector v2(v1);` v2��v1��һ��������

`vector v3(n, i);` v3����n��ֵΪi��Ԫ�ء�

`vector v4(n);` v4����ֵ��ʼ����Ԫ�ص�n��������
#### deque
deque�ṩ����������ṹ�� ��һ����ȫ������vector������ʵ����������һ��ά����������λ��ַ��

deque���˾���vector�����й����⣬ ��֧�ָ�Ч����/β�˲���/ɾ���������ڹ����Ϻϲ���vector��list��
#### list
ʲôfw
### priority_queue
`priority_queue<int> q` �൱�� `priority_queue<int, vector<int>, less<int> > q;` Ĭ�ϴ󶥶�

`priority_queue<int, vector<int>, greater<int> > q;` С����

## ���
### �������
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
���ָ�����ȣ�N�����ַ���, ����ʱ�ضϣ����ƴ�СM��, ����ʱ�Ҷ��루�ո���䣩`printf("%N.Ms", str);`

��Ϊ�����`printf("%-N.Ms", str);`

 ?
N,M���Զ�ָ̬������ \* ����M����N��Ȼ���ڲ����б������һ�����ֲ�����

���ӣ�`printf("%-*.*s", 5,2,"123");`

### cout

��?`<iomanip>`?�����˶���������Ŀ��ơ�

`cout << fixed << setprecision(3) << i << endl; // ����С����󾫶�`

## ����
(��Լ����ʱ��) `cin = getline(cin) > scanf > getchar > fgets`

��`scanf`����`string`��`s.resize(n); scanf("%s", s);`

��ʱs��С��Ϊn��������Զ���䡣
### ����ֵ
`fgets(char *str, MAX_SIZE, stdin);`���ȡ�����滻�з�������ʱ���ؿ�ָ��( 0 )

`getline(cin, str);` `cin.getline(char *str, MAX_SIZE);`����ǰ�ÿո񣬲����������ַ�������EOF����0

`>>`�������غ�������EOF����0

`scanf`����ֵΪ�����ַ������ļ���β����EOF
## ���ֺ���
### �ڽ�����
```
__builtin_xxxll(n) // long long ��

__builtin_ffs(n) // ĩβ1��λ�� (ctz + 1)
__builtin_clz(n) // ǰ��0����
__builtin_ctz(n) // ĩβ0����

__builtin_popcount(n) // 1�ĸ���
__builtin_parity(n) // 1�ĸ�������ż (popcount % 2)
```
### memset��ʼ��
```
memset(a,0x3F,sizeof(a)); //set int to 1061109567
memset(a,0x7F,sizeof(a)); //set int to 2139062143

memset(a,0xc0,sizeof(a)); //set int to -1061109568
memset(a,0x80,sizeof(a)); //set int to -2139062144

memset(a,0x7F,sizeof(a)); //set double to 1.38242e+306
memset(a,0xFE,sizeof(a)); //set double to -5.31401e+303
```

## ����ϸ��
λ�����߱�����������ٶȣ����������㶮��

`std::vector<bool>`��Ч�ʻ��������`std::bitset`

ÿ�ε��ú���ʱ�������´��������βΣ����ô����ʵ�ζ��βν��г�ʼ��
��ʵ�α�ֵ����ʱ�൱�ڴ���ʵ�εĿ�������������������Ͷ��������������Ƚϵ�Ч
������vector������һ���ܳ���string���������������ı��βε�ֵ����ý�������Ϊ�������á�

`inline int cmp(string a,string b) {return a<b;}`

��Ϊ
`inline int cmp(const string &a,const string &b) {return a<b;}`
# �ַ���
## �ַ����йغ���
### std::string��
#### ��ȡ��

`s.substr(pos, size);` posΪ�±꣬sizeΪ��ȡ����

#### ��ת��

`reverse(s.begin(), s.end());` ԭ�ط�ת

`s1.assign(s.rbegin(), s.rend());` ��ת������s1

#### ���ģ�

`s.assign(str);`

`s.assign(str, pos, size);`

`s.assign(str, pos,string::npos);` ���ַ���str��pos��ʼ����β����s

`s.assign("nico", 5);` �ѡ�n�� ��I�� ��c�� ��o�� ��\0�������ַ���

`s.assign(n, 'x');` �����x�����ַ���

#### ɾ����

`s.erase(pos);` ������pos��ʼ����ȫɾ��

`s.erase(pos, n);` ������pos��ʼ����ɾn��

`iterator erase(iterator it);` ɾ��itָ����ַ�������ɾ�����������λ��  

`iterator erase(iterator first, iterator last);` ɾ��[first��last��֮��������ַ�

`s.erase(std::remove(s.begin(), s.end(), 'a'), s.end());` ɾ�������ض��ַ�

`s.erase(std::unique(s.begin(), s.end()) ,s.end());`
ɾ�������ظ��ַ���Ҫ��s����

#### ���ң�

`s.find('c');` ��0��ʼ�����ַ�c�ڵ�ǰ�ַ�����λ��

`s.find(str, pos);` ��pos��ʼ����str�ڵ�ǰ���е�λ��

`s.find(str, pos, n);` ��pos��ʼ����str��ǰn���ַ��ڵ�ǰ���е�λ��

#### ��Сдת����

`transform(s.begin(), s.end(), s.begin(), ::toupper);`

`transform(s.begin(), s.end(), s.begin(), ::tolower);`

### ����/�ַ���ת��
#### ��������(���͡������͡������͵�)������ת��Ϊ�ַ���

`itoa()`��������ֵת��Ϊ�ַ�����
`itoa(int n, char* s, int r)  //��10����nת��Ϊr���Ʋ�����s`

`ltoa()`����������ֵת��Ϊ�ַ�����

`ultoa()`�����޷��ų�����ֵת��Ϊ�ַ�����

`gcvt()`������������ת��Ϊ�ַ�����ȡ�������롣

`ecvt()`����˫���ȸ�����ֵת��Ϊ�ַ�����ת������в�����ʮ����С���㡣

`fcvt()`��ָ��λ��Ϊת�����ȣ�����ͬecvt()��

#### ���ַ���ת��Ϊ��������(���͡������͡������͵�)

������`const char *nptr, char **endptr, int base`

�磺`str.c_str(), NULL, 10`

`atof()`�����ַ���ת��Ϊ˫���ȸ�����ֵ��

`atoi()`�����ַ���ת��Ϊ����ֵ��

`atol()`�����ַ���ת��Ϊ������ֵ��

`strtod()`�����ַ���ת��Ϊ˫���ȸ�����ֵ�������治�ܱ�ת��������ʣ�����֡�

`strtol()`�����ַ���ת��Ϊ����ֵ�������治�ܱ�ת��������ʣ�����֡�

`strtoul()`�����ַ���ת��Ϊ�޷��ų�����ֵ�������治�ܱ�ת��������ʣ�����֡�

## KMP
### next����
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
void quick_get_nex(int l) // �Ż���next���飬����ģ�壬��̫���
{
    int i = 1, j = 0; // ��1��ʼ
    nex[1] = 0;
    while (i < l)
    {
        if (j == 0 || m[i - 1] == m[j - 1]) //-1��Ϊ�˺�string ���㿪ʼ���Ӧ
        {
            i++, j++;
            if (m[i - 1] != m[j - 1]) // ��ͬ���еĵ�һλ��ͬλ�����л��ݣ����Ѿ���ͬ�ˣ�����֮���λ�õ�Ԫ��������ͬ��
            {
                nex[i] = j;
            }
            else
                nex[i] = nex[j]; // ����һ��������ıȽ�
        }
        else
            j = nex[j];
    }
}
```
### ������ģʽ����ƥ��
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
### next�����������
1. ��� i \% ( i - next[ i ] ) == 0, ���ַ��� 1 ~ i - next[ i ]Ϊ����С��ѭ���ڡ����Ҵ��ַ�����ѭ���ڸ������Ϊ i / (i - next[ i ]);
   
2. ��� i - next[ ? ]�ܹ��õ� i % ( i - next[ ? ] ) == 0�� �� i - next[ ? ]ҲΪ��ѭ���ڣ���������С��ѭ���ڡ���ѭ���ڵĸ���Ϊ i / ( i - next[ ? ] );
   
3. ��� i % ( i - next[ i ] ) != 0, ����ַ����������ܹ���������ַ�����ѭ���ڡ�

4. next[ i ] ����,ѭ����δ�ش���: �� abcabcab ��, next[ 8 ] = 5,?���Ƕ��� i ���Բ�����ѭ���ڡ����� i - next[ i ] ��Ϊ�ظ��ģ�ֻ������һ���޷���ȫ����������8 - 5 == 3, ����Ϊ abc,abc ����ѭ����
## Manacher
```
std::string s, str;
int p[N];
void add() { // ��֤�ַ�������Ϊ����
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
        while (str[i + p[i]] == str[i - p[i]]) p[i]++; // ������չ
        if (i + p[i] > r) { // �����ҷ�Χ
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
## �ַ�����ϣ
$P$ һ��ȡ $13331$ �� $23333$��$MOD$ Ϊ $1e9+7$ �� $1e9+9$
### ��Ȼ���
```
typedef unsigned long long ULL;
char s[N];
ULL hash[N], p[N]; // MODΪULL�Ͻ磬���׹�ϣ��ͻ��������
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
### ˫��ϣ
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
## AC�Զ���
```
char s[N];
char str[N];
struct node {
    int son[26];
    int cnt;
    int fail;
} nod[N];
int cnt;
void insert(int len) { // ��Trie��
    int pos = 0;
    for (int k = 0; k < len; k++) {
        int t = s[k] - 'a';
        if (nod[pos].son[t] == 0) nod[pos].son[t] = ++cnt;
        pos = nod[pos].son[t];
    }
    nod[pos].cnt++;
}
void build() { // ����failָ��
    std::queue<int> q;
    for (int i = 0; i < 26; i++) // ��һ��ڵ����
        if (nod[0].son[i]) q.push(nod[0].son[i]);
    while (!q.empty()) {
        int t = q.front(); q.pop(); // ��ǰ�ڵ�
        int fail_t = nod[t].fail; // ��ǰ�ڵ�ʧ��ָ��Ľڵ�
        for (int i = 0; i < 26; i++) {
            int pos = nod[t].son[i]; // ��ǰ�ڵ���ӽڵ�
            int fail_pos = nod[fail_t].son[i]; // ʧ��ڵ��ͬ�ӽڵ�
            if (pos) {
                nod[pos].fail = fail_pos; // ���ֽڵľ͸����ֽڵ��fail
                q.push(pos);
            }
            else {
                nod[t].son[i] = fail_pos; // û�ӽڵ������ֽڵ�
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
        while (pos) { // ��ÿ���ڵ��ʧ��ָ���ж�
            res += nod[pos].cnt;
            if (nod[pos].cnt) nod[pos].cnt = 0;
            pos = nod[pos].fail;
        }
    }
    return res;
}
```
### AC�Զ����ϵĶ�̬�滮
Trieͼ��ÿ���㶼��һ��״̬����AC�Զ�����״̬�໥ת��������ʵ�ֶ�̬�滮

1. ��ģʽ����ԭ���г��ֵĴ�����������ģʽ����ͬ��
   
    ����Trie��ʱ����ÿ������β�ڵ���б�ǣ���AC�Զ�����ƥ��ʱ��ÿ����һ�ν�β�ڵ㼴��ʾ�ɹ�ƥ�䣬ans++

2. ��ģʽ����ԭ���г��ִ�������ģʽ��B��ģʽ��A���Ӵ�����ֻ��¼A��
   
    ����Trie��ʱ����ÿ������β�ڵ���б�ǣ���AC�Զ�����ƥ��ʱ���Ծ������Ӵ�ģʽ��������ǣ�������֮ǰ��ͬ

3. ��ԭ���в������κ�ģʽ���Ĵ�����������
   
    ������ģʽ������AC�Զ�����ģʽ������ֹ�Ķ��ǷǷ��ģ����ɾ���dp[i][j]��ʾ����Ϊi��״̬Ϊj���ַ�������������ö�������ַ�����״̬ƥ�䣬�𰸼�Ϊsum(dp[m][i])��m��С��n�ϴ󣬿��Կ����þ���˷�����dp

4. ��һ��������̵Ĵ�ʹ������������ģʽ����
   
    ������ģʽ������AC�Զ�������n��С������״̬ѹ��dp
    ������״̬ $j\&2^k$ ��ʾ�Ӹ��ڵ㵽�ý���·�����е�k��ģʽ�� $dp[i][j]$ ��ʾ״̬ i ��������״̬j����̳��ȣ���ʼ�� $dp[i][j] = 0;$
    ��Trie���� $(0,0)$ ��$(i,2^{n-1})$������·���𰸼��� $dp[i][2^{n-1}]$
# ���ݽṹ
## ���鼯
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
## ��������
### ֱ��
����dfs��ֱ��
```
std::vector<int> e[N];
int max, ith; // ��󳤶ȺͶ˵�
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
### ����
����������**���ڵ���** **��С**�Ľڵ��Ϊ��������
```
std::vector<int> e[N];
int min = INF, ith;
int dfs(int pos, int fa) {
    int dep = 1;
    int res = 0; // ���ڵ��������������ڵ���
    for (auto i : e[pos]) {
        if (i == fa) continue;
        int t = dfs(i, pos); // ��ǰ�����ڵ���
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
for (int i = 1; i <= n; i++) // Ԥ���������λ��������log2(i) + 1��
        lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
```
```
std::vector<int> e[N];
int lg[N];
int fa[N][M], dep[N];
void dfs(int pos, int fath) { // ��¼��Ⱥ�����
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
        a = fa[a][lg[dep[a] - dep[b]] - 1]; // ע���һ��
    if (a == b) return a;
    for (int i = lg[dep[a]] - 1; i >= 0; i--) // �Ӵ�С��
        if (fa[a][i] != fa[b][i])
            a = fa[a][i], b = fa[b][i];
    return fa[a][0];
}
```
### ���
���õݹ���ձ�ǣ���Ϊ���µ���·��Ψһ

����ÿ���������++���������������յ�--��������������
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
#### ����
���������������
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
#### �߲��
���������������
1. `a -> lca`
2. `b -> lca`
```
int res = lca(a, b);
p[a]++;
p[b]++;
p[res] -= 2;
```
## ST��
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
## ��״����
`inline int lowbit(int x) { return x & -x; }`
### �����޸ģ������ѯ
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
### �����޸ģ������ѯ
�����ԭ����Ĳ�����飬Ȼ������״����ά����ת��Ϊ�����޸ģ������ѯ��

���ò����������ʣ���ǰ׺�ͼ�Ϊ��Ԫ�ر���
```
update(l, x, n);
update(r + 1, -x, n);
```
### �����޸ģ������ѯ
���������� $sum1 [ n ], sum2 = sum1 [ i ] * i;$ ( i?��1��ʼ���� )
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
        res += (p + 1) * sum1[i] - sum2[i]; // �ص㣡
    return res;
}
ll range_getsum(ll l, ll r) {
    return getsum(r) - getsum(l - 1);
}
```
## �߶���
```
struct tree {
    int l, r, s, lz;
} t[N << 2]; // ���ı�
int a[N];
inline int gls(int i) { return i << 1; } // ����
inline int grs(int i) { return i << 1 | 1; } // ����
void build(int i, int l, int r) { // ����
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
void push_down(int i) { // �����
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
void add(int i, int x, int y, int k) { // �����޸�
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
int search(int i, int x, int y) { // �����ѯ
    if (x <= t[i].l && t[i].r <= y) return t[i].s;
    push_down(i);
    int ls = gls(i), rs = grs(i);
    int res = 0;
    if (t[ls].r >= x) res += search(ls, x, y);
    if (t[rs].l <= y) res += search(rs, x, y);
    return res;
}
```
# ��̬�滮
## ˼·
��һ��dp״̬�Ķ��壬**�����Ѿ�**�õ��������ӽṹ��ȥ�����ܷ������ý���״̬ת��

����ʼ������ת�ƹ��̷ֿ����ۣ���ֹȫ�ֿ���ʱ���ڸ��ӵ���˼·����
## ����
### �ڲ�ѭ���������Ż�������V�ϴ�������

```
for (int i = 1; i <= n; i++)
	cin >> c[i] >> w[i], s[i] = s[i - 1] + c[i];
for (int i = 1; i <= n; i++) {
	int bound = max(c[i], V - (s[n] - s[i]));
	for (int j = V; j >= bound; j--)
		f[j] = max(f[j], f[j - c[i]] + w[i]);
}
```
### �������ı���ģ��
```
int dp[N], last[N];
void solve()
{
    int n = read(), V = read(); // n����������V��Ǯ��
    for (int i = 1; i <= n; i++)
    {
        memcpy(last, dp, (V + 1) << 2); // ��Ҫ�̳���һ����Ʒ������������ɱ���ʹ�ö�ά����
        int v = read(), m = read();    // v�������ã�m��������
        for (int k = 1; k <= m; k++)
        {
            int c = read(), w = read(); // c�������á�w������ֵ
            for (int j = V - v; j >= c; j--)
                last[j] = max(last[j], last[j - c] + w);
        }
        for (int j = v; j <= V; j++)
            dp[j] = max(dp[j], last[j - v]); // f����Ϊ���������ķ��ú������ֵ
    }
    printf("%d\n", dp[V]);
}
```
## ״ѹdp
### tricks
#### ����һ�����ϵ��Ӽ�
����һ������ $S$ ��`for (int i = S; i; i = (i - 1) & S)`

��״̬ת�ƣ�`dp[S] = std::max(dp[S], dp[S ^ i] + s[i]);`
## ��λdp
### ��λdp����˼·��ժ����ȣ�
```
dfs(�����������λ, ������������, ��ǰ�ڼ�λ)
	if ���һλ
    	return �������������µķ���ֵ
    �ֲ����� ct = ��ǰλ������
    �ֲ����� sum = 0;
    for i = 0 to ct - 1
    	sum += ��ǰλȡiʱһ���������ƵĺϷ�״̬��
        sum += ��ǰλȡiʱ���㵱ǰ���ƵĺϷ�״̬��
    ����ct������������ ����������return sum
    return sum + dfs(��ǰλ�������λ,���º����������,��һλ)

slv(��ǰ��)
	if(ֻ��һλ) return ��Ӧ�Ĺ���
    �ֲ����� ct;
    for ct = �������λ to 1
    	if ��ǰλ������ break
    �ֲ����� nw = ��ǰλ����
    �ֲ����� sum = 0
    for i = 1 to nw - 1
    	sum+=��ǰλȡi��Ϸ��������ȡ�Ĺ���
    for i = 1 to ct - 1
    	for j = 1 to 9
        	sum += ��iλȡj��Ϸ��������ȡ�Ĺ���
    sum += dfs(ȥ����һλ�������λ,��������,�ڶ�λ)
    return sum

main
	Ԥ����ǰλȡi�ĸ��������������ƵĹ���
    ���� L R
    --L
    ��� slv(R) - slv(L)
    return 0
```
# ͼ��
## ��/��ͼ
### ��ʽǰ���� $O(m)$
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
����ѭ�����������ڵ�����Ըõ�Ϊ�������бߣ�
`for (int i = head[x]; i; i = e[i].next)`

���뽻������
`if (dis[e[i].to] > dis[x] + e[i].weight)`
## ��С������
### Prim ����ͼ $O(n^2)$
```
int dis[N], vis[N], edge[N][N];
int Prim(int pos) {
	memset(dis, 0x3f, sizeof dis);
	dis[pos] = 0;
	for (int i = 1; i <= n; i ++) {
		int cur = 0;
		for (int j = 1; j <= n; j ++) {
			if (!vis[j] && (!cur || dis[j] < dis[cur])) {
				cur = j; // Ѱ��һ��Ȩֵ��С�ĵ�, ��¼�±�
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
### Prim ���Ż� $O(nlogn)$
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
### Kruskal ϡ��ͼ $O(mlogm)$
```
struct edge {
	int u, v, w;
} r[N];
bool cmp(const edge &a, const edge &b) {
	return a.w > b.w;
}
int fa[N]; // ���ò��鼯
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
## ���·
### Dijkstra $O(n^2)$
```
for (int j = 1; j <= n; j ++ )
    if (!vis[j] && (t == -1 || dis[t] > dis[j])) // �����ҵ���ǰ��̽ڵ�t
       t = j;
vis[t]=1;
for (int j = 1; j <= n; j ++ )
    dis[j] = min(dis[j], dis[t] + e[t][j]); // ����t����dis
```
### Dijkstra ���Ż� $O(nlogn)$
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
	q.push(1); // ��ʱ1ΪԴ�㣬����Դ��ͽ�ȫ������룬����ʼ��dis����
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
					return 1; // ��������1
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
	for (int k = 1; k <= n; k ++ ) // ����ѭ�����¸�����֮����룬kΪ�м�ý��
		for (int i = 1; i <= n; i ++ )
			for (int j = 1; j <= n; j ++ )
				dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
}
```
# ��ѧ
## ���Դ���
### ����
```
struct Matrix {
    int a[N][N];
    inline void init() { // �����
		memset(a, 0, sizeof a);
	}
	inline void unit() { // ��λ����
		init();
		for (int i = 0; i < N; i++) a[i][i] = 1;
	}
};
```
#### ��������
```
//���ؾ���ӷ�
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
//���ؾ������
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
//���ؾ���˷�
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
//���ؾ���ȽϷ�
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
#### ���������
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
### ��˹��Ԫ
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
## �����ѧ
### �������
```
int fac[N], inv[N]
void init(int n) { // Ԥ������Ԫ
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
# ����
## ����
### ֪ʶ��
#### ŷ����������
1. �� $i$ Ϊ����, $o[i] = i - 1;$

2. �� $i$ Ϊ����, $n \% i == 0, o[i * n] = o[n] * i;$

3. �� $n$ �� $i$ ����, $o[i * n] = o[n] * o[i];$

### ����ɸ
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
### ����ɸͬʱ��ŷ������
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
				o[i * primes[j]] = o[i] * primes[j]; // ����2
				break;
			}
			o[i * primes[j]] = o[i] * (primes[j] - 1); // ����3
		}
	}
}
```
### �󵥸�ŷ������
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
### ŷ���������Ч��ͬ����ɸ�棩
```
int E[N];
void n_euler_q(int n) // ŷ���������
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
### Miller_Rabin��������
```
bool MRtest(LL n) {
    if (n < 3 || n % 2 == 0)
        return n == 2; // ����
    LL u = n - 1, t = 0;
    while (u % 2 == 0)
        u /= 2, ++t;
    const LL ud[] = {2, 325, 9375, 28178, 450775, 9780504, 1795265022};
    for (LL a : ud) {
        LL v = qpow(a, u, n);
        if (v == 1 || v == n - 1 || v == 0)
            continue;
        for (int j = 1; j <= t; j++) {
            v = qmul(v, v, n); // ��Ҫ�÷�long long����Ŀ�����
            if (v == n - 1 && j != t) {
                v = 1;
                break;
            } // ����һ��n-1�����涼��1��ֱ������
            if (v == 1)
                return 0; // �������ǰ��û�г���n-1����⣬���μ���ʧ��
        }
        if (v != 1)
            return 0; // Fermat����
    }
    return 1;
}
```
## ͬ��
### ֪ʶ��
#### ����С����
��� $p$ ��һ�������������� $a$ ���� $p$ �ı���, ���� $a ^{ p - 1 } \equiv 1 \pmod p$
### ��չŷ�����
```
int exgcd(int a, int b, int &x, int &y) // ��չŷ�����
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
### �˷���Ԫ
#### ʹ�÷�Χ��
1. $n\perp p$ ����Ԫ�Ĵ��������������޽�
2. $p$ ����Ϊ��������չŷ�����
3. Ҫ�� $p$ Ϊ����������С���� `qpow(n, m - 2, m)`������������������
```
int inv(int n, int m) // n�ĳ˷���Ԫ
{
	int x, y;
	exgcd(n, m, x, y);
	return (x % m + m) % m; // ����ΧԼ����[0,m-1]����
}
```
#### ��������Ԫ
```
int inv[N];
void n_inv(int n, int m) // [1,n] ��������Ԫ��ʹ�÷�Χͬ����С����
{
	inv[1] = 1;
	for (int i = 2; i <= n; ++i)
	{
		inv[i] = (-1 * m / i + m) * inv[m % i] % m;
	}
}
```
### ͬ�෽����
```
struct func {
    int m, r; // ��mȡģ��r
} f[N];
```
#### �й�ʣ�ඨ��
```
int crt(int n) {
    int M = 1;
    for (int i = 0; i < n; i++)
		M *= f[i].m; // Ҫ��ģ�����ʣ�MΪ��С������
    int res = 0;
    for (int i = 0; i < n; i++) {
        int t = M / f[i].m;
        res = (res + f[i].r * t * inv(t, f[i].m)) % M;
    }
    return res;
}
```
#### ��չ�й�ʣ�ඨ��
```
int excrt(int n) {
    int M = f[0].m;
    int A = f[0].r;
    for (int i = 1; i < n; ++i) {
        int x = 0, y = 0;
        int ch = f[i].r - A;
        int r = exgcd(M, f[i].m, x, y);
        if (ch % r) return -1;
        A = ch / r * x * M + A; //�˷�����������ɿ���__int128���߿��ٳ�
        M = M / r * f[i].m;
        A = (A % M + M) % M;
    }
    return A;
}
```
## ɢװ����
### ������
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
### ���ٳ�
```
LL qmul(LL a, LL b, LL mod) {
    LL c = (long double)a / mod * b;
    LL res = (ULL)a * b - (ULL)c * mod; // �����������
    return (res + mod) % mod;
}
```
### ������� $O(log(a + b))$
```
int gcd(int a, int b) {
	return b ? gcd(b, a % b) : a;
}
```
### ��С������
```
int lcm(int a, int b) {
	return a / gcd(a, b) * b;
}
```
### Ψһ�ֽⶨ��
```
int num[N], prime[N]; // primes����Ϊɸ�õ�����
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
���ۣ����� $N = p_{1}^{c_{1}}p_2^{c_{2}}...p_{k}^{c_{k}}$ 
1. $N$ ��ĳ����Լ�����Ա�ʾ�ɣ�$d = p_{1}^{a_{1}}p_2^{a_{2}}...p_{k}^{a_{3}}, \quad(0 \leq a_{i} \leq c_{i},i \in[1, k])$

2. $N$ ����Լ������Ϊ��$(c_{1}+1) * (c_{2}+1) * ... * (c_{k}+1)$

3. $N$ ��������Լ��֮��Ϊ�� $(1+p_{1}+p_{1}^2+..+p_{1}^{c_{1}}) * (1+p_{2}+p_{2}^2+..+p_{2}^{c_{2}})*... * (1+p_{k}+p_{k}^2+..+p_{k}^{c_{k}})$������12��������Լ��Ϊ1��2��3��4��6��12����ô������Լ��֮��Ϊ1 + 2 + 3 + 4 + 6 + 12 = 28����
