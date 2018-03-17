# 为什么使用二分查找，而不是三分查找？

考虑平均比较次数

## 对于二分查找：

存在的比较情况：

- 比较middle是否等于target
- 比较middle是否小于target

单次查找平均比较次数：$\frac{1 + 2}{2} = \frac{3}{2}$

$$
T(1) = 1 
$$

$$
  T(n) = T(\frac{n}{2}) + \frac{3}{2}
$$

$$
  T(\frac{n}{2}) = T(\frac{n}{4}) + \frac{3}{2}
$$

$$
T(n) = T(\frac{n}{4}) + \frac{3}{2} \cdot 2
$$

$$
\therefore T(n) = T(1) + \frac{3}{2} \cdot \log_2{n} = 1 + \frac{3}{2} \cdot \log_2{n}
$$

## 对于三分查找：

存在的比较情况：

- 比较middle是否等于target_1
- 比较middle是否小于target_1
- 比较middle是否等于target_2
- 比较middle是否小于target_2

单次查找平均比较次数：$\frac{1 + 2 + 3 + 4}{4} = \frac{5}{2}$

$$
T(1) = 1 
$$

$$
  T(n) = T(\frac{n}{3}) + \frac{5}{2}
$$

$$
  T(\frac{n}{3}) = T(\frac{n}{9}) + \frac{5}{2}
$$

$$
T(n) = T(\frac{n}{9}) + \frac{5}{2} \cdot 2
$$

$$
\therefore T(n) = T(1) + \frac{5}{2} \cdot \log_3{n} = 1 + \frac{5}{2} \cdot \log_3{n}
$$

## 对比：

$$
 1 + \frac{3}{2} \cdot \log_2{n} < 1 + \frac{5}{2} \cdot \log_3{n}
$$

所以，二分查找优于三分查找。

对于x分查找

单次平均比较次数：

$$
x - \frac{1}{2}
$$

平均比较次数

$$
f(x) = 1 + (x-\frac{1}{2})\cdot\log_x{n}
$$

可以证明 $f(x)$在$(2,+\infty)$是增函数，从而证明二分查找由于n分查找$(n > 2)$