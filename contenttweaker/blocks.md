# 方块

与物品一样，你需要先创建一个 `BlockBuilder`。

导入：`import mods.contenttweaker.block.BlockBuilder;`

## 新建 BlockBuilder

与 `ItemBuilder` 不同，它的构造函数需要一个 `MCMaterial` 参数。游戏里有哪些 `MCMaterial`，你依然可以用 `/ct dump` 查看。

`new BlockBuilder(<blockmaterial:earth>);`

> 这个参数可以省略，会取默认值 `<blockmaterial:iron>`

## 设定属性

你可以用以下方法设定属性，这些方法也是一样是链性调用的。

| 方法及参数 | 例子 | 描述 |
| ---- | ----- | ---- |
| `notSolid()` | `.notSolid()` | 如果你的方块模型不是完整方块，或者有透明的地方，一定要调用这个方法以避免你的方块变成 X 光方块。 |
| `setRequiresTool()` | `.setRequiresTool()` | 设定方块需要工具才能挖掘。|
| `withHardnessAndResistance(hardnessAndResistance as float)` | `.withHardnessAndResistance(0.5f)` | 同时设置硬度和抗爆等级 |
| `.withHardnessAndResistance(hardnessIn as float, resistanceIn as float)` | `.withHardnessAndResistance(0.5f, 0.5f)` | 设置硬度和抗爆等级 |
| `.withHarvestLevel(harvestLevel as int)` | `.withHarvestLevel(3)` | 挖掘等级 |
| `.withHarvestTool(harvestTool as ToolType)` | `.withHarvestTool(<tooltype:shovel>)` | 挖掘工具 |
| `.withItemGroup(group as MCItemGroup)` | `.withItemGroup(<itemgroup:building_blocks>)` | 创造标签 |
| `.withLightValue(lightValueIn as int)` | `.withLightValue(15)` | 亮度 |
| `.withLootFrom(blockIn as MCBlock)` | `.withLootFrom(<block:minecraft:diamond>)` | 设定该方块的掉落与什么相同，CoT 还是会自动生成这个方块的默认战利品表，但是会被游戏忽略 |
| `withMaxStackSize(maxStackSize as int)` | `.withMaxStackSize(16)` | 一组多少个物品？（默认值 64）|
| `withRarity(rarity as string)` | `.withRarity("EPIC")` | 设置物品稀有度，会影响游戏内物品名字的颜色，可使用 COMMON UNCOMMON RARE EPIC |
| `withRenderType(renderType as BlockRenderType)` | `.withRenderType(BlockRenderType.TRANSLUCENT);` | 设置方块[渲染类型](https://docs.blamejared.com/1.16/en/mods/contenttweaker/API/block/BlockRenderType/)，如果设定的不是 SOLID，同时还会调用 `notSolid` 方法。
| `withSlipperiness(slipperinessIn as float)` | `.withSlipperiness(0.5f);` | 滑度 |
| `withTickRandomly()`| `.withTickRandomly()` | 方块会收到随机刻 |
| `withoutDrops()` | `.withoutDrops()` | 方块无掉落，CoT 还是会自动生成这个方块的默认战利品表，但是会被游戏忽略 |
| `withoutMovementBlocking()` | `.withoutMovementBlocking()` | 设置方块不会被活塞推动，同时还会调用 `notSolid` 方法。 |
| `withType(T : BlockTypeBuilder) as T` | `.withType<BlockBuilderPillarRotatable>()` | 将方块设置为其他特殊类型的方块，可进行进一步的参数设置，这个方法调用后前面的方法将不可用 |

## 注册方块

当你所有参数都设置完了，可以调用 `build(name as string)` 方法来注册物品，参数为这个方块的注册名，只允许包含阿拉伯数字、小写字母和下划线 `_`。

## lang key

lang key 为 `block.contenttweaker.方块名`

## 例子

```kotlin
#loader contenttweaker

import mods.contenttweaker.block.BlockBuilder;
import mods.contenttweaker.block.stairs.BlockBuilderStairs;
import mods.contenttweaker.block.basic.BlockBuilderBasic;
import mods.contenttweaker.block.pillar.BlockBuilderPillarRotatable;


// 最简单的方块，Material 为默认值 <blockmaterial:iron>
new BlockBuilder()
    //Will delegate to the Basic Builder
    .build("generic_block");


// 设定 Material
new BlockBuilder(<blockmaterial:earth>)
    .build("earth_like_block");


// 柱子类似的方块，类似原木
// 和原木一样有 xyz 轴三种方向
// 材质默认名字为 "block_name" + "end", "_sides"
new BlockBuilder()
    .withType<BlockBuilderPillarRotatable>()
    .build("preset_pillar_rotatable_noarg");


// 楼梯
// 需要三个材质, 顶部, 底部, 侧面, 默认名为 "block_name" + "_top", "_bottom", "_sides"
new BlockBuilder()
    .withType<BlockBuilderStairs>()
    .build("stairs_noarg");
```
