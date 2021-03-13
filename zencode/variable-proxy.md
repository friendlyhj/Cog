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

## Setter

变量不可能只能 get，更需要 set，需要修改。我们可以使用代理类来保存数据。然而 ZenCode 目前不能使用静态字段，所以我们需要实例化（即使用 `new` 关键字创建一个对象）这个类。

```javascript
public class ProxyTwo {
    public this() {} // 无参数构造器

    public var foo as int = 18;
}

var proxyTwo = new ProxyTwo();

// 然而这个实例化的对象也是变量，我们还需要代理它

public class ProxyTwoHandler {
    public function get() => proxyTwo;
}

// 调用
// 先用 ProxyTwoHandler.get() 获取它的实例
// 再获取这个实例的字段
print(ProxyTwoHandler.get().foo); // get
ProxyTwoHandler.get().foo = 42; // set
print(ProxyTwoHandler.get().foo)
```
