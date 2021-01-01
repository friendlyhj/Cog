# lambda 表达式

lambda 表达式取代了 1.12 的匿名函数。

## 格式

例如 1.12 的 `IRecipeFunction` 格式为

```javascript
function(out, ins, info) {
    return out;
}
```

在 1.15+ 被替换成（虽然 1.15+ 的配方函数不是这样的了，只是举个例子）

```javascript
(out, ins, info) => {
    return out;
}
```

上面的是块 lambda 表达式，如果大括号内的语句只有一条的话，可以改成行 lambda 表达式。上面的例子便可以改成……

```javascript
(out, ins, info) => out
```

## 用途

lambda 表达式是一个对象，所以它可以用做方法的参数又或是作为变量。当一个方法的参数为“函数式接口”（Functional Interface）时，就说明这里需要填入一个 lambda 表达式。lambda 表达式的参数和返回值不可乱填，他们的类型和具体意义由函数式接口决定。