# 物品转换器

想在配方当中使用工具又不想吞工具？你就需要使用物品转换器了！物品条件和物品转换器也可以同时使用！

注意：1.16 的物品转换器只适用于不可堆叠的物品，对可堆叠物品使用会产生刷物品 Bug！

## 创建

使用 IIngredient 的 `transformCustom` 方法。这个方法有两个参数：

* uid as `string`：物品转换器的 ID，ID 要相互独立。建议如果要用多次带有物品转换器的物品，先把他存进变量里。
* function as `Function<IItemStack, IItemStack>`：一个以 IItemStack 为参数（具体的物品），返回值为 IItemStack （作为转换后的物品）的 lambda 表达式。用于确定物品转换器的具体逻辑。

```javascript
// 参与合成后，钻石镐变成铁镐
<item:minecraft:diamond_pickaxe>.transformCustom("level_down", (stack) => <item:minecraft:iron_pickaxe>);
```

## 复用

我们可以复用已经创建的物品转换器。

```javascript
val a = <item:minecraft:diamond_pickaxe>.transformCustom("level_down", (stack) => <item:minecraft:iron_pickaxe>);

// 现在这个金镐参与合成后也会变成铁镐了
<item:minecraft:golden_pickaxe>.transform(a.transformer);
```

## 内置物品转换器

以下是 CrT 内置的物品转换器。

* `reuse()`：不会消耗
* `transformDamage(amount as int)`：合成后掉几点耐久
* `transformReplace(replaceWith as IItemStack)`：合成后会变成什么物品

```javascript
<item:minecraft:diamond_axe>.reuse();
<item:minecraft:diamond_axe>.transformDamage(5);
<item:minecraft:diamond_pickaxe>.transformReplace(<item:minecraft:iron_pickaxe>);
```
