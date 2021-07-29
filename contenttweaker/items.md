# 物品

首先，你需要创建一个 `ItemBuilder`，你需要先导入：

`import mods.contenttweaker.item.ItemBuilder;`

## 新建 ItemBuilder

使用 `new` 关键词，新建一个 `ItemBuilder`

`new ItemBuilder()`

## 设定属性

你可以用以下方法设定物品的属性。设定属性是 *链性调用* 的。例子本章最后一节。

| 方法及参数 | 例子 | 描述 |
| ---- | ----- | ---- |
| `isImmuneToFire()` | `.isImmuneToFire()` | 设置为防火，与下界合金相同 |
| `withFood(food as MCFood)` | `.withFood(new MCFood(2, 0.2f))` | 设置这个物品为食物，第一个参数为回多少饥饿度，第二个参数是[营养价值](https://minecraft.fandom.com/zh/wiki/%E9%A3%9F%E7%89%A9#.E8.90.A5.E5.85.BB.E4.BB.B7.E5.80.BC)的一半，MCFood 还有更多内容可设置，见对应文档 |
| `withItemGroup(itemGroup as MCItemGroup)` | `.withItemGroup(<itemGroup:misc>)` | 设置在哪个创造标签下（默认在杂项） |
| `withMaxDamage(maxDamage as int)` | `.withMaxDamage(250)` | 设定最大耐久值，设置后将强制使物品不可堆叠（即 maxStackSize = 1） |
| `withMaxStackSize(maxStackSize as int)` | `.withMaxStackSize(16)` | 一组多少个物品？（默认值 64）|
| `withNoRepair()` | `.withNoRepair();` | 物品不可在铁砧上修复 |
| `withRarity(rarity as string)` | `.withRarity("EPIC")` | 设置物品稀有度，会影响游戏内物品名字的颜色，可使用 COMMON UNCOMMON RARE EPIC |
| `withType(T : ItemTypeBuilder) as T` | `.withType<ItemBuilderTool>()` | 将物品设置为其他特殊类型的物品，可进行进一步的参数设置，这个方法调用后前面的方法将不可用 |

当你用 `.withType<ItemBuilderTool>()` 转换为工具后，你可以设置工具相关的参数，所有方法及其例子在下面的例子都写了，不多赘述。

## 注册物品

当你所有参数都设置完了，可以调用 `build(name as string)` 方法来注册物品，参数为这个物品的注册名，只允许包含阿拉伯数字、小写字母和下划线 `_`。

## lang key

lang key 为 `item.contenttweaker.物品名`

## 例子

```java
#loader contenttweaker
import mods.contenttweaker.item.ItemBuilder;
import mods.contenttweaker.item.tool.ItemBuilderTool;
import crafttweaker.api.food.MCFood;

// 创建一个最简单的物品
new ItemBuilder().build("generic_item");
new ItemBuilder().build("generic_item_2");
new ItemBuilder().build("generic_item_3");

// 设定属性
new ItemBuilder()
    .withMaxStackSize(16) // 16 个一组
    .withFood(new MCFood(2, 0.2f)) // 设定为食物
    .isImmuneToFire() // 防火
    .build("generic_item_4");

// 创建工具
new ItemBuilder()
    .withMaxDamage(100) // 耐久
    .withType<ItemBuilderTool>() // 将物品转换为工具，进行进一步关于工具的参数设定
    .withAttackDamage(10.0F) // 伤害值
    .withAttackSpeed(5.0F) // 攻击速度
    .withDurabilityCostAttack(1) // 用它打怪掉几点耐久，默认值为 2，一般普通工具为 2，剑为 1
    .withDurabilityCostMining(1); // 用它挖方块掉几点耐久，默认值为 0
    .withToolType(<tooltype:pickaxe>, 2, 3.0f) // 镐，挖掘等级为 2，挖掘速度为 3.0
    .build("my_tool");
```
