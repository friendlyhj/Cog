# 空类型安全

在物品章节中，已经提到过 IItemStack 是 Nonnull 了。ZenCode 引入了空类型安全，从而尽可能规避臭名昭著的空指针异常。（对 null 调用任何方法将抛出的异常，而 Java 的所有引用对象均有可能为 null）

## 具体解释

ZenCode 的所有对象默认都不可能为 null。如果这个对象有可能为 null，则需要在这个对象的类名后面加个问号。如 `IItemStack?`。此外 1.12 的 isNull 函数不复存在。如果你需要检查一个对象是否为 null。只需要 `obj == null` 即可。

## bug

目前可空类型不能使用拓展类引入的方法，而 CrT 内部大量使用了拓展类，所以导致很多方法对可空类不可用。往往你需要在用 if 排除空的情况后，将变量强转到非空类。

```javascript
val world as MCWorld? = ....;
if (world != null) {
    val nonNullWorld as MCWorld = world as MCWorld;
    // 两边都出现了 as XXX
    // 但意义不同
    // 左边的是将这个变量指定为 MCWorld 非空类型
    // 右边的是将 MCWorld? 强转为 MCWorld
    println(nonNullWorld.gameTime); // 调用方法（好吧，这是 getter）
}
```
