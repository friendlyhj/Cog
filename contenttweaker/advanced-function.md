# 高级运用

CoT 允许创建一些高级的物品/方块，这些物品/方块能有一些自定义的特定功能。

## 创建

使用 `ItemBuilder#withType` 和 `BlockBuilder#withType` 可指定要创建的物品/方块为高级物品/方块。

```csharp
#loader contenttweaker
import mods.contenttweaker.item.ItemBuilder;
import mods.contenttweaker.item.advance.ItemBuilderAdvanced;
import mods.contenttweaker.block.BlockBuilder;
import mods.contenttweaker.block.advance.BlockBuilderAdvanced;

new ItemBuilder()
    .withType<ItemBuilderAdvanced>() // 设置为高级物品
    .build("inf_flint_and_steel");

new BlockBuilder()
    .withType<BlockBuilderAdvanced>() // 设置为高级方块
    .build("test_block");
```

## 设置具体逻辑

由于游戏具体逻辑的执行是在游戏加载完才会执行，所以具体功能逻辑的设置是在 CrT 脚本指定的。

```kotlin
// 再强调一遍应该在 CrT 脚本内设置
#loader crafttweaker

// 在 CoT 脚本创建完高级物品方块后
// 你可以用如下的尖括号引用来获取 CoTItemAdvanced 和 CoTBlockAdvanced 对象
// 通过这个对象的 setter 你可以设置它们的功能
// <advanceditem:inf_flint_and_steel>
// <advancedblock:test_block>

// 让这个物品就像个无限耐久的打火石
// 这个函数会在用该物品右键方块时触发
<advanceditem:inf_flint_and_steel>.setOnItemUse((context) => {
    val pos = context.pos;
    val direction = context.direction;
    val firePos = pos.offset(direction);
    val world = context.world;
    if (world.isAir(firePos)) {
        world.setBlockState(firePos, <blockstate:minecraft:fire>);
    }
    return ActionResultType.SUCCESS;
});
```

高级物品方块有哪些设置，具体可以看官方文档。

[CoTBlockAdvanced](https://docs.blamejared.com/1.16/en/mods/contenttweaker/API/block/advance/CoTBlockAdvanced)
[CoTItemAdvanced](https://docs.blamejared.com/1.16/en/mods/contenttweaker/API/item/advance/CoTItemAdvanced)
