# switch

switch-case 是一种特殊的条件语句，用于给程序设置分支。

基本格式：

```javascript
switch(expression) { // 依次匹配 expression 的结果和哪一个 case 相同，并执行该 case 下的语句
    case a: // a 必须是字面值，不能是变量，且只允许 byte int short char 和 string
        // do something
        break; // 用于退出 switch 程序块
    case b:
        // [statement]
        break;
    // 可以写无限个 case
    default: // 如果都不匹配，则执行这个，default 必须放到 switch-case 程序块最下面
        // do something
}
```

## Multi-case

当程序匹配到 case 后，会执行 switch 块内该 case 下（和其余 case）的所有内容。所以大部分分支最后面都需要 `break` 来退出 switch-case 程序块。比如如下的程序：

```javascript
switch(character) {
    case "A": 
        println("a");
    case "B":
        println("b");
    case "D":
        println("d");
    case "E":
        println("e");
    default:
        println("default");
}
```

如果 `character` 是 B 的话，则会依次打印 b d e 和 default。这往往是我们不想看见的，所以记得每个分支最后加上 `break`！

但你也可以稍稍改一下下...

```javascript
switch(expression) { // 依次匹配 expression 的结果和哪一个 case 相同，并执行该 case 下的语句
    case a: 
        // do something
        break;
    case b:
    case c: // 当 expression 为 b 或 c 时执行下面的语句
        // [statement]
        break;
    default:
        // do something
}
```

```javascript
var grade as string = "B";

switch(grade) {
    case "A": 
        println("excellent");
        break;
    case "B":
    case "C": 
        println("good");
        break;
    case "D":
        println("pass");
        break;
    case "E":
        println("fail");
        break;
    default:
        println("unknown grade");
}
```
