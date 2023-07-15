# 动态规划
## 思路
对一个dp状态的定义，**假设已经**得到了最优子结构，去考虑能否完整得进行状态转移

将初始条件和转移过程分开讨论，防止全局考虑时过于复杂导致思路受阻
## 背包
### 内层循环的下限优化（适用V较大的情况）

```cpp
for (int i = 1; i <= n; i++)
	std::cin >> c[i] >> w[i], s[i] = s[i - 1] + c[i];
for (int i = 1; i <= n; i++) {
	int bound = std::max(c[i], V - (s[n] - s[i]));
	for (int j = V; j >= bound; j--) {
		f[j] = std::max(f[j], f[j - c[i]] + w[i]);
    }
}
```
### 有依赖的背包模板
```cpp
int dp[N], last[N];
void solve() {
    int n = read(), V = read(); // n主件个数，V总钱数
    for (int i = 1; i <= n; i++) {
        memcpy(last, dp, (V + 1) << 2); // 需要继承上一组物品的最优情况，可避免使用二维数组
        int v = read(), m = read();    // v主件费用，m附件个数
        for (int k = 1; k <= m; k++) {
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