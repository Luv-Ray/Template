# 数据结构
## 并查集
```cpp
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
```cpp
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
```cpp
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
```cpp
for (int i = 1; i <= n; i++) { // 预处理二进制位数，等于log2(i) + 1
        lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
}
```
```cpp
std::vector<int> e[N];
int lg[N];
int fa[N][M], dep[N];
void dfs(int pos, int fath) { // 记录深度和祖先
    fa[pos][0] = fath;
    dep[pos] = dep[fath] + 1;
    for (int i = 1; i <= lg[dep[pos]]; i++) {
        fa[pos][i] = fa[fa[pos][i - 1]][i - 1]; // 2 = 1 + 1
    }
    for (auto i : e[pos]) {
        if (i != fath) dfs(i, pos);
    }
}
int lca(int a, int b) {
    if (dep[a] < dep[b]) std::swap(a, b);
    while (dep[a] > dep[b]) {
        a = fa[a][lg[dep[a] - dep[b]] - 1]; // 注意减一！
    }
    if (a == b) return a;
    for (int i = lg[dep[a]] - 1; i >= 0; i--) { // 从大到小减
        if (fa[a][i] != fa[b][i])
            a = fa[a][i], b = fa[b][i];
    }
    return fa[a][0];
}
```
### 差分
利用递归回收标记，因为从下到上路径唯一

对于每条链：起点++（包含递增），终点--（不包含递增）
```cpp
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
```cpp
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
```cpp
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
```cpp
int max[N][M];
int query(int l, int r) {
    int k = log2(r - l + 1);
    return std::max(max[l][k], max[r - (1 << k) + 1][k]);
}
```
```cpp
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
```cpp
void update(int x, int k, int n) {
	for (int i = x; i <= n; i += lowbit(i)) {
		t[i] += k;
    }
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
```cpp
update(l, x, n);
update(r + 1, -x, n);
```
### 区间修改，区间查询
构造差分数组 $sum1 [ n ], sum2 = sum1 [ i ] * i;$ ( i 从1开始计数 )
```cpp
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
```cpp
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

