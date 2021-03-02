# 物品条件

有时候普通的物品还不足以使用，可能需要一些特殊的合成，只有在完全满足对应条件下才会工作。

## 创建

使用 IIngredient 的 `onlyIf` 方法。这个方法有两个参数：

* uid as `string`：物品条件的 ID，ID 要相互独立。建议如果要用多次带有物品条件的物品，先把他存进变量里。
* function as `Predicate<IItemStack>`：一个以 IItemStack 为参数，返回值为 bool 的 lambda 表达式。用于确定怎样的具体物品才能参与合成。

再说一遍，标签不是 IIngredient，要想使用对物品标签使用物品条件，你必须先用 `asIIngredient` 方法把标签转换成材料！

```javascript
// 只有含有 Display NBT 的铁锭才能参与合成

<tag:items:forge:ingots/iron>.onlyIf("helloworld", (stack) => {
    return stack.tag.contains("Display");
})
```

## CrT 对耐久的额外支持

对于耐久，CrT 有两个内置的物品条件，我们可以直接使用：`onlyDamaged` 和 `anyDamage`。

```javascript

<item:minecraft:iron_pickaxe>.anyDamage(); // 任何耐久的铁镐
<item:minecraft:diamond_pickaxe>.onlyDamaged(); // 有损耗的钻石镐
```
