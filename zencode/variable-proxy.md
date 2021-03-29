# 变量代理

ZenCode 取消了全局静态变量，然而 `public` 关键词只能用于类（和其的字段）和函数，不能用于变量。针对这个问题，本章使用「变量代理」来处理它。

> 变量代理不是 ZenCode 的特性，而是一个 tip

## Getter 函数

我们可以添加一个函数，这个函数只返回这个变量。别的脚本调用这个函数来获取这个变量。

```javascript
val foo as int = 123;

public function getFoo() as int {
    return foo;
}

// 可以改成 lambda 表达式
public function getFoo() as int => foo;

// 调用这个全局函数
print(getFoo());
```

## 代理类

然而使用全局 getter 函数，可能会被脚本优先级所影响，且直接全局也显得不太优雅。我们也可以给一个类添加一个静态方法，来返回一个变量。

```javascript
public class Proxy {
    public static getFoo() as string => "foo";
}

// 调用
print(Proxy.getFoo());
```

## 静态字段

我们可以在代理类新建一个静态字段，这个字段可以随意修改。

```javascript
public class Proxy {
    public static var foo as int = 233; // 再提一遍，var 代表这个变量可以重新赋值，而 val 不能。
}

// get
print("variable foo: " + Proxy.foo);
// set
Proxy.foo = 333;
print("now, variable foo: " + Proxy.foo);
```

请注意，变量代理均使用静态字段和函数，所以不要用 `new` 实例化这个类。
