# 预处理器

以 `#` 开头的注释会被 ZenCode 特殊处理，将其视为「预处理器」。ZenCode 语言本身不会对这些注释进行任何特殊处理，而 CraftTweaker 则添加了部分预处理器来处理它们。虽然如果找不到合适的预处理器，该行会被当成注释处理，但依旧有可能产生未定义的行为。尽量使用 `//` 来书写注释。

## 简单预处理器

简单预处理器只需要一个单词，没有任何参数。

| 名称 | 作用 |
|:--- | :--- |
| `#debug` | 脚本编译后的字节码文件将会输出到 `classes` 文件夹内 |
| `#noload` | 该脚本将不再加载 |
| `#loadfirst` | 该脚本将第一个加载 |
| `#loadlast` | 该脚本将最后一个加载 |
| `#nobrand` | 脚本重载时将不再显示 CrT 赞助者 |

## 带参数的预处理器

带参数的预处理器需要在其名字后加参数。

| 名称 | 参数 | 参数默认值 | 作用 | 使用例子 |
|:--- | :--- | :--- | :--- | :--- |
| `#loader` | 脚本加载器名 | `crafttweaker` | 指定脚本加载器，默认为 `crafttweaker` | `#loader contenttweaker` |
| `#priority` | 脚本优先级 | `10` | 指定脚本优先级 | `#priority 100` |
| `#replace` | 将被替换的字符，替换的字符 | 无 | 将脚本的部分字符替换掉 | `#replace ture true` （这样 ture 就能当 true 用了） |

## Snip 预处理器

Snip 预处理器需要一个开始预处理器和一个结束预处理器（`#snip end`）。在这两个预处理器中间的脚本代码将视为一个片段。根据开始预处理器，将决定这些片段会不会执行。

### 开始预处理器

开始预处理器有三个：

* `#snip start`：该片段肯定会执行
* `#snip modloaded <mod1> [mod2] [mod3] [modN]`：只有指定的模组全部安装上才会执行。
* `#snip modnotloaded <mod1> [mod2] [mod3] [modN]`：只有指定的模组都没安装上才会执行。

### 例子

```zenscript
#snip modloaded mekanism

print("mekanism loaded!");

#snip end

print("out side of snip");

#snip modnotloaded mekanism

print("mekanism not loaded!");

#snip end
```
