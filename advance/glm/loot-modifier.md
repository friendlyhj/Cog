# 战利品修饰符

ILootModifier（战利品修饰符）(`crafttweaker.api.loot.modifiers.ILootModifier`) 是一个函数式接口，他有两个参数：

* `loot as List<IItemStack>` 已经由战利品表抽奖出的结果
* `currentContext as LootContext` 战利品表抽奖的当前背景

这个函数需要返回一个 `List<IItemStack>` 来确定实际的战利品表抽奖结果。

你可以对这个 List 进行删改，然后依旧返回这个 List。亦或是返回一个新的 List 从而完全接管这个战利品表。

```less
// 这个修饰符由于直接返回了 loot list，而并没有修改，所以实际上没啥用
(loot, currentContext) => loot

(loot, currentContext) => {
    // 给 loot list 添加了铁锭
    // 相当于给战利品表抽奖结果额外添加了一个铁锭
    loot.add(<item:minecraft:iron_ingot>);
    return loot;
}

// 直接忽略原来的战利品抽奖结果，使实际上的抽奖结果固定为一个铁锭
// 数组可以自动转型为 List，这是没有问题的
(loot, currentContext) => [<item:minecraft:iron_ingot>]
```

## CommonLootModifiers

写 lambda 表达式可能有些麻烦，CrT 提供了 CommonLootModifiers 工具类来快速构建所需的战利品修饰器。

### 导入

使用前最好先导入它：

`import crafttweaker.api.loot.modifiers.CommonLootModifiers;`

### 可用条件

```less
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.loot.modifiers.ILootModifier;

// 返回给战利品表抽奖结果添加一个指定物品的修饰器
// CommonLootModifiers.add(stack as IItemStack) as ILootModifier

val addIronIngot as ILootModifier = CommonLootModifiers.add(<item:minecraft:iron_ingot>);

// 返回给战利品表抽奖结果添加多种指定物品的修饰器
// CommonLootModifiers.addAll(stack as IItemStack[]) as ILootModifier

val addIronIngotAndApple as ILootModifier = CommonLootModifiers.add([<item:minecraft:iron_ingot>, <item:minecraft:apple>]);

// 返回删除所有战利品表抽奖结果的修饰器
// CommonLootModifiers.clearLoot() as ILootModifier

val clearLoot as ILootModifier = CommonLootModifiers.clearLoot();

// 返回删除指定物品的修饰器
// CommonLootModifiers.remove(target as IIngredient) as ILootModifier)
// 参数为 IIngredient，所以我们可以填物品标签进去
val removeRedstone as ILootModifier = CommonLootModifiers.remove(<tag:items:forge:dusts/redstone>);

// 返回删除多种物品的修饰器
// CommonLootModifiers.removeAll(targets as IIngredient[]) as ILootModifier
val removeIronIngotAndPoppy as ILootModifier = CommonLootModifiers.removeAll([<tag:items:forge:ingots/iron>, <item:minecraft:poppy>]);

// 返回将一种物品替换为另一种物品的修饰器
// 该修饰器会对物品数量进行匹配
// 对于将两个胡萝卜替换成一个马铃薯的修饰器，当它作用于结果为五个胡萝卜的战利品表，替换结果为
// 两个马铃薯和一个胡萝卜。
// 因为每两个胡萝卜被替换为一个马铃薯，剩余一个胡萝卜则没有替换
// CommonLootModifiers.replaceStackWith(target as IItemStack, replacement as IItemStack) as ILootModifier
val replaceTwoCarrotsWithOnePotato as ILootModifier = CommonLootModifiers.replaceStackWith(<item:minecraft:carrot> * 2, <item:minecraft:potato>);

// 返回将一种物品替换为另一种物品的修饰器
// 由于该修饰器使用 IIngredient，所以我们可以使用物品标签，但也没有了数量的指定，只会一换一
//CommonLootModifiers.replaceWith(target as IIngredient, replacement as IItemStack) as ILootModifier

val replaceCarrotWithPotato as ILootModifier = CommonLootModifiers.replaceWith(<item:minecraft:carrot>, <item:minecraft:potato>);

// 与前面的同名的方法功能一样，只不过参数是映射，表示了多个替换规则
// CommonLootModifiers.replaceAllStacksWith(replacementMap as IItemStack[IItemStack]) as ILootModifier
// CommonLootModifiers.replaceAllWith(replacementMap as IItemStack[IIngredient]) as ILootModifier

// 将多个修饰器连接起来组成一个新的修饰器
// CommonLootModifiers.chaining(modifiers as ILootModifier[]) as ILootModifier

// 例子调用了前面定义的修饰器，则为先删除全部再加个铁锭
val clearThenAddIronIngot as ILootModifier = CommonLootModifiers.chaining([clearLoot, addIronIngot]);
```
