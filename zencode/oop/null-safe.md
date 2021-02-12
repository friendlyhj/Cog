# 空类型安全

在物品章节中，已经提到过 IItemStack 是 Nonnull 了。ZenCode 引入了空类型安全，从而尽可能规避臭名昭著的空指针异常。（对 null 调用任何方法将抛出的异常，而 Java 的所有引用对象均有可能为 null）

## 具体解释

ZenCode 的所有对象默认都不可能为 null。如果这个对象有可能为 null，则需要在这个对象的类名后面加个问号。如 `IItemStack?`。此外 1.12 的 isNull 函数不复存在。如果你需要检查一个对象是否为 null。只需要 `obj == null` 即可。
