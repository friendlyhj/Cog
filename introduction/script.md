# 脚本

### ZenCode

CraftTweaker 提供了一个脚本语言来进行操作。在 1.12 及其以前的 MC 版本中这个语言叫做 ZenScript。而在 1.14 后 ZenScript 被 ZenCode 取代，语法与先前有所不同，也比之前提供了更多的特性。在 ZenCode 章节会对其进行更多的解释。

### scripts 文件夹

CraftTweaker 会读取所有游戏根目录（即 .minecraft 文件夹）下的 scripts 路径（及其子文件夹）下的所有 zs 文件。这是个纯文本文件，你可以使用任何文本编辑器编辑它（不要是记事本就好）。文件编码应设置为 UTF-8。

**目前 ZenCode 不支持中文，脚本内不能出现中文字符，否则会报错！**

### println 语句

1.12- 的 print 语句在 1.15+ 改名为 `println`

```javascript
println("hello, world");
```

println 语句将会把里面的内容打印到 logs/crafttweaker.log 中，这个文件是 crafttweaker 的日志文件。

写好这个语句，保存，在游戏内输入 `/reload` 指令重载语句。然后你就可以在 logs/crafttweaker.log 中看见这么一行：

```text
[16:06:52.867][DONE][CLIENT][INFO] hello, world
```

这就是 CrT 日志显示格式了：

```text
[时:分:秒:毫秒][游戏加载阶段][脚本加载在客户端还是服务端][日志发送等级，信息、警告还是错误] 日志信息
```

### 注释

注释能使你的脚本更易读。

单行注释以// 或 \#（建议用//，\#可能与预处理器冲突）开头，区块注释以`/*`开头，`*/`结尾。

```javascript
// A comment
// print("green");  <= Comment a statement
# Also a comment
/* A
Block
Comment
*/
```

