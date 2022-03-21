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

| 方法与参数 | 返回值 | 作用 |
| :--- | :--- | :--- |
| `indexOf(c as char)` | `usize?` | 返回该字符串第一个出现给定字符的索引，如果该字符串没有该字符，则返回 `null` |
| `indexOf(c as char, from as int` | `usize?` | 返回该字符串从给定索引开始数第一个出现给定字符的索引，如果该字符串没有该字符，则返回 `null` |
| `indexOf(s as string)` | `usize?` | 返回该字符串第一个出现给定字符串的索引，如果该字符串没有给定字符串，则返回 `null` |
| `indexOf(s as string, from as int` | `usize?` | 返回该字符串从给定索引开始数第一个出现给定字符串的索引，如果该字符串没有给定字符串，则返回 `null` |
| `lastIndexOf(c as char)` | `usize?` | 返回该字符串最后一个出现给定字符的索引，如果该字符串没有该字符，则返回 `null` |
| `lastIndexOf(c as char, until as int)` | `usize?` | 返回该字符串直到给定索引为止最后一个出现给定字符的索引，如果该字符串没有该字符，则返回 `null` |
| `lastIndexOf(s as string)` | `usize?` | 返回该字符串最后一个出现给定字符串的索引，如果该字符串没有给定字符串，则返回 `null` |
| `lastIndexOf(s as string, until as int)` | `usize?` | 返回该字符串直到给定索引为止最后一个出现给定字符串的索引，如果该字符串没有给定字符串，则返回 `null` |
| `split(delimiter as char)` | `string[]` | 将字符串分割成多个字符串，参数为分隔符 |
| `trim()` | `string` | 将字符串左右的空白字符（空格，制表符，换行等）删除 |
| `startsWith(head as string)` | `bool` | 是否以给定字符串开头 |
| `endsWith(head as string)` | `bool` | 是否以给定字符串结尾 |
| `lpad(length as usize, c as char)` | `string` | 如果该字符串长度不足给定长度，则在左边加上给定字符补齐 |
| `rpad(length as usize, c as char)` | `string` | 如果该字符串长度不足给定长度，则在右边加上给定字符补齐 |
| `matchesRegex(regex as string)` | `bool` | 字符串是否匹配给定正则表达式 |
| `wrap(with as string, escape as bool)` | `string` | 给字符串左右两边加上 with 字符串，如果 `escape` 参数为 `true`，则会同时把字符串的控制字符转义 |
| `quoteAndEscape()` | `string` | 给字符串左右两边加上双引号，同时把字符串的控制字符转义 |

### 示例

```kotlin
var str = "This is a test string!";

println(str.length); // 长度

if ("es" in str) { // 检测一个字符串是否包含另一个字符串
  println(str + " contains es!");
}

if (str.indexOf("es") != null) {
  println(str  + " contains 'es' at position: " + str.indexOf("es"));
}

for st in str.split(" ") {
  println(st); // 依次打印 This, is, a, test, string!
}

println(str[1]); // 获取字符串指定索引的字符，将会打印 h
println(str[1 .. 4]); // 获取字符串子串，索引左开右闭，将会打印 his
```
