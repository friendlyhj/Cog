# 拓展类

拓展类允许为一个已经存在的类添加更多方法。在很多情况下，能够代替函数。

## 例子

该例子为 IItemStack 添加了 show 方法，使用它便可以在日志打印出该物品的 ID，以 CrT 尖括号形式。

```java
import crafttweaker.api.item.IItemStack;

public expand IItemStack {
    public show() as void {
        println(this.registryName.commandString);
    }
}
```

expand 后面即为我们想要拓展的类名，然后你就可以简单的为这个类添加新的方法了。

之后你就可以使用 `<item:minecraft:iron_ingot>.show()` 这样调用你创建的拓展方法了~
