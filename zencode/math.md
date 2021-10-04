# 数学函数

ZenCode StdLib 还引入了大量的数学函数可供使用。

## 导入包

使用前你需要进行导入

`import math.Functions;`

## 可用函数

| 方法名 | 参数 | 返回值 | 作用 |
| :--- | :--- | :--- | :--- |
| `sin` | `double` | `double` | 正弦（弧度制，下同） |
| `cos` | `double` | `double` | 余弦 |
| `tan` | `double` | `double` | 正切 |
| `asin` | `double` | `double` | 反正弦 |
| `acos` | `double` | `double` | 反余弦 |
| `atan` | `double` | `double` | 反正切 |
| `sinh` | `double` | `double` | 双曲正弦 |
| `cosh` | `double` | `double` | 双曲余弦 |
| `tanh` | `double` | `double` | 双曲正切 |
| `toRadians` | `double` | `double` | 角度制转换成弧度制 |
| `toDegrees` | `double` | `double` | 弧度制转换成角度制 |
| `exp` | `double` | `double` | 以自然常数为基底的幂 |
| `log` | `double` | `double` | 自然对数 |
| `log10` | `double` | `double` | 常用对数 |
| `sqrt` | `double` | `double` | 平方根 |
| `cbrt` | `double` | `double` | 立方根 |
| `ceil` | `double` | `double` | 向上取整 |
| `floor` | `double` | `double` | 向下取整 |
| `abs` | `double/int` | `double/int` | 绝对值 |
| `signum` | `double` | `double` | 符号函数，正数返回 1.0，负数返回 -1.0，零返回零 |
| `min` | `int/long/float/double` | `int/long/float/double` | 取最小值 |
| `max` | `int/long/float/double` | `int/long/float/double` | 取最大值 |
| `round` | `double/float` | `long` | 四舍五入取整 |
| `pow` | `double, double` | `double` | 返回第一个数的第二个数次幂 |

## 数学常数

导入包：`import math.Constants;`

可用 `Constants.PI` 与 `Constants.E` 获取圆周率和自然常数。

## 例子

```kotlin
import math.Functions;
import math.Constants;

print(Constants.PI); // 打印 3.141592653589793
print(Functions.sin(3.14)); // 打印 0.0015926529164868282
```
