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
			c.a[i][j] = (a.a[i][j] + b.a[i][j]) % MOD;
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
			c.a[i][j] = ((a.a[i][j] - b.a[i][j]) % MOD + MOD) % MOD;
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
				c.a[i][j] += a.a[i][k] * b.a[k][j] % MOD;
				c.a[i][j] %= MOD;
			}
		}
	}
	return c;
}
//重载矩阵比较符
bool operator==(const Matrix &a, const Matrix &b) {
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			if (a.a[i][j] != b.a[i][j]) {
				return 0;
			}
		}
	}
	return 1;
}
bool operator!=(const Matrix &a, const Matrix &b) {
	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			if (a.a[i][j] != b.a[i][j]) {
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