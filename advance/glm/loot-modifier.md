# 战利品修饰器

**战利品修饰器只是描述了如何修改战利品表。你写好一个修饰器，还需要注册它，确定它会影响那些战利品表后才会生效！如何注册见后面的章节！**

ILootModifier（战利品修饰器）(`crafttweaker.api.loot.modifiers.ILootModifier`) 是一个函数式接口，他有两个参数：

* `loots as List<IItemStack>` 已经由战利品表抽奖出的结果
* `currentContext as LootContext` 战利品表抽奖的当前背景

这个函数需要返回一个 `List<IItemStack>` 来确定实际的战利品表抽奖结果。

你可以对这个 List 进行删改，然后依旧返回这个 List。亦或是返回一个新的 List 从而完全接管这个战利品表。

```kotlin
// 这个修饰符由于直接返回了 loot list，而并没有修改，所以实际上没啥用
(loots, currentContext) => loots

(loots, currentContext) => {
    // 给 loot list 添加了铁锭
    // 相当于给战利品表抽奖结果额外添加了一个铁锭
    loots.add(<item:minecraft:iron_ingot>);
    return loots;
}

// 直接忽略原来的战利品抽奖结果，使实际上的抽奖结果固定为一个铁锭
// 数组可以自动转型为 List，这是没有问题的
(loots, currentContext) => [<item:minecraft:iron_ingot>]
```

## CommonLootModifiers

写 lambda 表达式可能有些麻烦，CrT 提供了 CommonLootModifiers 工具类来快速构建所需的战利品修饰器。

### 导入

使用前最好先导入它：

`import crafttweaker.api.loot.modifiers.CommonLootModifiers;`

### 可用条件

```kotlin
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.loot.modifiers.ILootModifier;

// 返回给战利品表抽奖结果添加一个指定物品的修饰器
// CommonLootModifiers.add(stack as IItemStack) as ILootModifier

val addIronIngot as ILootModifier = CommonLootModifiers.add(<item:minecraft:iron_ingot>);

// 返回给战利品表抽奖结果有几率添加一个指定物品的修饰器
// CommonLootModifiers.addWithChance(stack as MCWeightedItemStack) as ILootModifier
// 物品 % 几率
val addIronIngotHalfChance = CommonLootModifiers.addWithChance(<item:minecraft:iron_ingot> % 50);

// 返回给战利品表抽奖结果添加多种指定物品的修饰器
// CommonLootModifiers.addAll(stack as IItemStack[]) as ILootModifier

// 返回给战利品表抽奖结果添加多种指定物品与概率的修饰器
// CommonLootModifiers.addAllWithChance(stacks as MCWeightedItemStack[]) as ILootModifier
val foo = CommonLootModifiers.addAllWithChance([<item:minecraft:honey_bottle> % 50, <item:minecraft:dried_kelp> % 13]);

// 添加物品，数量为两个值之间，均匀分布
// CommonLootModifiers.addWithRandomAmount(stack as IItemStack, min as int, max as int) as ILootModifier
val abc = CommonLootModifiers.addWithRandomAmount(<item:minecraft:conduit>, 2, 9);

// 添加物品，但根据特定附魔等级，会额外再多掉落一些物品。如果你需要支持时运什么的，请用这个
// 物品数量与附魔等级的关系有三种算法
// 二项分布，原版用于煤矿石和红石矿石等
// 额外掉落数量为 附魔等级 + extra 和 p 的二项分布。
// CommonLootModifiers.addWithBinomialBonus(enchantment as MCEnchantment, extra as int, p as float, stack as IItemStack) as ILootModifier
val bar = CommonLootModifiers.addWithBinomialBonus(<enchantment:minecraft:fortune>, 3, 0.5714286, <item:minecraft:wheat_seeds>);
// 均匀分布
// 额外掉落数量为 0 至 附魔等级 × Multiplier 的均匀分布。
// CommonLootModifiers.addWithUniformBonus(enchantment as MCEnchantment, multiplier as int, stack as IItemStack) as ILootModifier
val xyz = CommonLootModifiers.addWithUniformBonus(<enchantment:minecraft:fortune>, 1, <item:minecraft:glowstone_dust>);
// 原版默认矿物掉落的分布，原版用于钻石矿石等
// 即有1/(附魔等级+2)几率数量改为原来的×(2至(附魔等级+1))，有2/(魔咒等级+2)几率不变。
val moreCoal = CommonLootModifiers.addWithOreDropsBonus(<enchantment:minecraft:fortune>, <item:minecraft:coal>);

val addIronIngotAndApple as ILootModifier = CommonLootModifiers.add([<item:minecraft:iron_ingot>, <item:minecraft:apple>]);

// 和上面的相同，只不过可以一次添加多种物品
// CommonLootModifiers.addAllWithBinomialBonus(enchantment as MCEnchantment, extra as int, p as float, stacks as IItemStack[]) as ILootModifier
// CommonLootModifiers.addAllWithOreDropsBonus(enchantment as MCEnchantment, stacks as IItemStack[]) as ILootModifier
// CommonLootModifiers.addAllWithUniformBonus(enchantment as MCEnchantment, multiplier as int, stacks as IItemStack[]) as ILootModifier

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

// 连接一个或多个修饰器，不过第一个已经确定好是清除原有物品的修饰器了
// CommonLootModifiers.clearing(modifiers as ILootModifier[]) as ILootModifier
// 这个和上面一个例子是等价的
val clearThenAddIronIngot2 as ILootModifier = CommonLootModifiers.clearing(addIronIngot);
```

## LootContext

LootContext 包含了当前战利品表的当前背景，他有这些 Getter。注意不是有些 Getter 返回的可能是 null。这也很好理解，对于方块掉落时的战利品抽奖，你不可能获取表示实体伤害类型的 DamageSource。

| Name | Type | Description |
|------|------|-------------|
| blockState | [MCBlockState](https://docs.blamejared.com/1.16/en/vanilla/api/block/MCBlockState)? | 当前破坏的方块状态 |
| damageSource | [DamageSource](https://docs.blamejared.com/1.16/en/vanilla/api/util/DamageSource)? | 造成当前实体死亡的伤害类型 |
| directKillerEntity | [MCEntity](https://docs.blamejared.com/1.16/en/vanilla/api/entity/MCEntity)? |  杀死当前实体的直接实体，如果玩家用箭射死一个实体，这个 Getter 返回的是箭 |
| explosionRadius | float | 造成方块破坏或实体死亡的爆炸的半径 |
| killerEntity | [MCEntity](https://docs.blamejared.com/1.16/en/vanilla/api/entity/MCEntity)? | 杀死这个实体的实体，注意会杀死实体的不只有玩家 |
| lastDamagePlayer | [MCPlayerEntity](https://docs.blamejared.com/1.16/en/vanilla/api/entity/MCPlayerEntity)? |  最后一次对该实体造成伤害的玩家 |
| lootTableId | [MCResourceLocation](https://docs.blamejared.com/1.16/en/vanilla/api/util/MCResourceLocation) | 当时的战利品表 ID |
| lootingModifier | int | 当时的战利品表修饰符 ID |
| luck | float | 玩家的幸运值 |
| origin | [MCVector3d](https://docs.blamejared.com/1.16/en/vanilla/api/util/MCVector3d)? |  Gets the origin, or location, of the loot roll, if present; null otherwise. |
| thisEntity | [MCEntity](https://docs.blamejared.com/1.16/en/vanilla/api/entity/MCEntity)? |  当前实体 |
| tileEntity | [MCTileEntity](https://docs.blamejared.com/1.16/en/vanilla/api/tileentity/MCTileEntity)? | 当前破坏方块内部的 TileEntity |
| tool | [IItemStack](https://docs.blamejared.com/1.16/en/vanilla/api/items/IItemStack) | 破坏方块所用的工具 |
| world | [MCServerWorld](https://docs.blamejared.com/1.16/en/vanilla/api/world/MCServerWorld) | 当前世界 |
| random | [Random](https://docs.blamejared.com/1.16/en/vanilla/api/util/Random/) | 进行战利品表抽奖用的随机数生成器 |

### 例子

```less
import crafttweaker.api.util.DamageSource;
import crafttweaker.api.loot.modifiers.ILootModifier;

val addGoldIngotWhenDiedByMagic as ILootModifier = (loots, currentContext) => {
    if (currentContext.damageSource != null) { // 确定非空
        val damageSource as DamageSource = currentContext.damageSource as DamageSource; // 获取当前 DamageSource，并强转为非空类型
        if (damageSource.magic) { // 如果是魔法
            loots.add(<item:minecraft:gold_ingot>);
        }
    }
    return loots; // 返回修改后的 loot list
};
```
