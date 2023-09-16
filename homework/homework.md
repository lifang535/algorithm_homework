---
### Q15
试确定函数 Mult（见程序 2-24）共执行了多少次乘法，该函数实现两个 n×n 矩阵的乘法。

```
// 2-24
template<class T>
void Mult(T **a, T **b, T **c, int n)
{ // 两个 nxn 矩阵 a 和 b 相乘得到 c
  for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++) {
      T sum = 0;
      for (int k = 0; k < n; k++)
        sum += a[i][k] * b[k][j];
      c[i][j] = sum;
    }
}
```

#### answer: 三层 for 循环，乘法执行 n * n * n 次。

---
### Q16
试确定函数 Mult (见程序 2-25) 共执行了多少次乘法，该函数实现一个 m×n 矩阵与一个 n×p 矩阵之间的乘法。

```
// 2-25
template<class T>
void Mult(T **a, T **b, T **c, int m, int n, int p)
{ // mxn 矩阵 a 与 nxp 矩阵 b 相乘得到 c
  for (int i = 0; i < m; i++)
    for (int j = 0; j < p; j++) {
      T sum = 0;
      for (int k = 0; k < n; k++)
        sum += a[i][k] * b[k][j];
    c[i][j] = sum;
    }
}
```

#### answer: 三层 for 循环，乘法执行 m * p * n 次。

---
### Q18
函数 MinMax(见程序2-26) 用来查找数组 a[0:n-1] 中的最大元素和最小元素。令 n 为实例特征。试问 a 中元素之间的比较次数是多少？程序 2-27 中给出了另一个查找最大和最小元素的函数。在最好和最坏情况下 a 中元素之间比较次数分别是多少？试分析两个函数之间的相对性能。

```
// 2-26
template<class T>
bool MinMax(T a[], int n, int& Min, int& Max)
{ // 寻找 a[0:n-1] 中的最小元素和最大元素
  // 如果数组中的元素数目小于 1，则返回 false
  if (n < 1) return false;
  Min = Max = 0; // 初始化
  for (int i = 1; i < n; i++) {
    if (a[Min] > a[i]) Min = i;
    if (a[Max] < a[i]) Max = i;
  }
  return true;
}
```

```
// 2-27
template<class T>
bool MinMax(T a[], int n, int& Min, int& Max)
{ // 寻找 a [0:n-1] 中的最小元素和最大元素
  // 如果数组中的元素数目小于1，则返回 f a l s e
  if (n < 1) return false;
  Min = Max = 0; // 初始化
  for (int i = 1; i < n; i++)
    if (a[Min] > a[i]) Min = i;
    else if (a[Max] < a[i]) Max = i;
  return true;
}
```

#### answer: 2-26 a 中元素的比较次数是 max{2 * (n - 1), 0}。2-27 最好情况：max{n - 1, 0}；最坏情况：max{2 * (n - 1), 0}。2-27 在部分情况下计算量小于 2-26，相对更优。

---
### Q20
程序 2-28 给出了另外一个迭代式顺序搜索函数。在最坏情况下 x 与 a 中元素之间执行了多少次比较？把这个比较次数与程序 2-1 中相应的比较次数进行比较，哪一个函数将运行得更快？为什么？

```
// 2-1
template<class T>
int SequentialSearch(T a[], const T& x, int n)
{ // 在未排序的数组 a[0:n-1]中搜索 x
  // 如果找到，则返回所在位置，否则返回- 1
  int i;
  for (i = 0; i < n && a[i] != x; i++);
  if (i == n) return -1 ;
  return i;
}
```

```
// 2-28
template<class T>
int SequentialSearch(T a[ ], const T& x, int n)
{ // 在未排序的数组 a[0:n-1]中查找 x
  // 如果找到，则返回相应位置，否则返回- 1
  a[n] = x; // 假定该位置有效
  int i;
  for (i = 0; a[i] != x; i++);
  if (i == n) return -1;
  return i;
}
```

#### answer: 最坏情况下：x 与 a 中元素之间执行了 n + 1 次比较。2-28 执行的更快，因为每次 for 循环 2-1 多了一个 `i < n` 的比较，增大了计算量。

---
### Q24
计算如下函数的平均执行步数：(1) SequentialSearch(见程序 2-2)；(2) SequentialSearch(见程序 2-28)；(3) Insert(见程序 2-10)

```
// 2-2
template <class T>
int SequentialSearch(T a[], const T& x, int n)
{ // 在未排序的数组 a[0:n-1] 中查找 x
  // 如果找到则返回所在位置，否则返回 -1
  if (n < 1) return -1;
  if (a[n-1] == x) return n - 1;
  return SequentialSearch(a, x, n - 1); # return 本身不计入执行步数
}
```
#### answer: 等可能的位置 i：0 ~ n - 1，执行步数：2 * (n - i)；未找到返回 -1，执行步数：2 * n + 1。
$$ Step_{avg} = \frac{1}{n + 1} (\sum_{i=0}^{n-1}{2 * (n - i)} + 2 * n + 1) =  \frac{n^2+3*n+1}{n+1}  $$

```
// 2-28
template<class T>
int SequentialSearch(T a[ ], const T& x, int n)
{ // 在未排序的数组 a[0:n-1] 中查找 x
  // 如果找到，则返回相应位置，否则返回 -1
  a[n] = x; // 假定该位置有效
  int i;
  for (i = 0; a[i] != x; i++); // 看看书 for 循环是怎么算执行步数的
  if (i == n) return -1;
  return i; // 看看书 return 是怎么算执行步数的
}
```
#### answer: 等可能的位置 i：0 ~ n - 1，执行步数：2 * (n - i)；未找到返回 -1，执行步数：2 * n + 1。

```
// 2-10
template<class T>
void Insert(T a[], int& n, const T& x)
{ // 向有序数组 a [0:n-1]中插入元素 x
  // 假定 a 的大小超过 n 
  int i;
  for (i = n-1; i >= 0 && x < a[i]; i--)
    a[i+1] = a[i];
  a[i+1] = x;
  n++; // 添加了一个元素
}

```

#### answer: 
