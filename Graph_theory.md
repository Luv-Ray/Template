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