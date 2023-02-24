# 数学
## 线性代数
### 矩阵
```
struct Matrix {
    int a[N][N];
    inline void init { memset(a, 0, sizeof a); }
	inline void unit {
		init();
		for (int i = 0; i < N; i++) a[i][i] = 1;
	}
};
```
#### 矩阵运算
```
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