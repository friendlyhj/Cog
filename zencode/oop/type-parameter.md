# 泛型

泛型，也可以叫类型参数。可以给强类型语言提供一点动态宽松的余地。在前文的 `List<T>` 中的 T 便是泛型。我们在使用时，则使用 `List<IItemStack>`，确定了这个类型参数，指定这个 List 只能存取这个类型的对象。

## 为什么需要泛型

以 `List` 为例。我们使用 `var list as List = new List()` 是合法的。但是这样的一个 List 则可以放入任何对象。在放入时，似乎是没有问题的。但在取出时，我们就不知道取出的对象是什么类型的了。诚然，我们可以用 `instanceof` 检查对象的类型和强制转换来解决问题。但这实在是比较繁琐了。所以泛型就出来了，在最开始就指定了这个 List 的具体类型。这样在存取时，我们就已经知道了对象的类型，不用在担心类型不匹配的问题了。

## 在 ZenCode 哪里用到了泛型

除了 List 外，数组和映射均有用到泛型，他们泛型形式则为 `T[]` 和 `V[K]`。

标签也用到了泛型，`MCTag<T>`，这个 T 就是这个标签存储了哪种对象。

## 使用泛型

往往我们使用带有泛型的类时，都需要在使用前指定好类型参数。如 `List<T>` 指定为 `List<IItemStack>`，确定这个 List 只能存 IItemStack，又如 `MCTag<MCItemDefinition>` 确定了这个标签是物品标签。但如果我想声明一个带有泛型的类或函数呢？

### 泛型类

直接把泛型写在类名后即可，用尖括号括起来。如 `public class Set<T>` `public class MultiMap<K, V>`

### 泛型方法

泛型方法目前只能声明在已经有泛型的类里的，并使用这个类的泛型。

```java
public expand V[K] {
    public getOptional(key as K) as V? {
        if (this has key) {
            return this[key];
        } else {
            return null;
        }
    }
}
```

注意，由于泛型的具体类型只有外部指定了才知道，所以内部实现是用不了标记为泛型的变量的方法的。

## MCTag 泛型

在学了泛型之后，我们终于可以了解标签的导入了。

```javascript
import crafttweaker.api.tag.MCTag;
import crafttweaker.api.item.MCItemDefinition;
import crafttweaker.api.fluid.MCFluid;
import crafttweaker.api.entity.MCEntityType;
import crafttweaker.api.blocks.MCBlock;

MCTag<MCItemDefinition> // 物品标签
MCTag<MCFluid> // 流体标签
MCTag<MCEntityType> // 实体标签
MCTag<MCBlock> // 方块标签
```
