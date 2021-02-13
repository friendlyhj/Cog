# 数据类型

`val / var` 定义变量时，程序会自动推断类型。但是在之后的使用中，这样会产生许许多多的问题，尽量不要省略类型名。那么，有多少类型呢？

请注意，以下类名为小写字母的开头的，都作为 ZenCode 的关键词，这些类无需导入。这些类便是 ZenCode 的基础类，他们作为一个编程语言的基础，底层上不依赖 Minecraft。

## 总览

数据类型为最最基础的，直接存储一个值的。它们没有任何方法、Getter、Setter可用。

| 类名 | 解释 | 示例 |
| :--- | :--- | :--- |
| 整型\(int\) | 任意整数\(范围为-2147483648~2147483647\) | `var test as int = 10;` |
| usize | 和 int 一样 (?) | `var test as usize = 14;` |
| 布尔值\(bool\) | 真\(`true`\)或假\(`false`\) | `var test as bool = true;` |
| 长整型\(long\) | 范围更大的整数\(一般int就够了\) | `var test as long = 2147483648;` |
| 单精度浮点数\(float\) | 小数 | `var test as float = 1.5;` |
| 双精度浮点数\(double\) | 也是小数，但是比float范围更大，有效数字更多 | `var test as double = 1.2345;` |
| 字符\(char\) | 一个字符 | `var test as char = 'a';` |
| 无类型\(void\) | 空，null，用于函数/方法表明该函数/方法无返回值 | 不可声明 `void 变量` |

Note：ZenCode 的 `as` 关键词可以用 `:` 替代。但为了沿袭 ZenScript，本教程依旧使用 `as`。

## 字符串

字符串在 Java 中并非数据类型，但在 zs 中也尤为重要。 声明字符串也很简单： `val test as string = "test";`

此外，字符串可以使用 `==` （与 `equals` 方法等价）、使用 `+` 操作符来拼接字符串，使用 `length` getter 获取字符串长度、`isEmpty` getter 检查是否为空字符串（`""`）。字符串同时也可以视为一个 `char` 数组。字符串有如下几个方法：

* `charAt`
* `trim`
* `toLowerCase`
* `toUpperCase`
* `split`
* `contains`