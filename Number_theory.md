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
void n_euler_q(int n) {
	for (int i = 2; i <= n; i++) {
		if (!E[i]) {
			for (int j = i; j <= n; j += i) {
				if (!E[j])
					E[j] = j;
				E[j] = E[j] / i * (i - 1);
			}
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
int exgcd(int a, int b, int &x, int &y) {
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
int inv(int n, int m) { // n的乘法逆元
	int x, y;
	exgcd(n, m, x, y);
	return (x % m + m) % m; // 将范围约束在[0,m-1]以内
}
```
#### 线性求逆元
```
int inv[N];
void n_inv(int n, int m) { // [1,n] 线性求逆元，使用范围同费马小定理
	inv[1] = 1;
	for (int i = 2; i <= n; ++i) {
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
