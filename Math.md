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
				c.m[i][j] += a.m[i][k] * b.m[k][j] % mod;
				c.m[i][j] %= mod;
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
			c.m[i][j] = (a.m[i][j] + b.m[i][j]) % mod;
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
			c.m[i][j] = ((a.m[i][j] - b.m[i][j]) % mod + mod) % mod;
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