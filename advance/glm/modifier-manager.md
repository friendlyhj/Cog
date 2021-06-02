# 战利品修饰器管理器

LootModifierManager 管理了所有的战利品表修饰器，前两章的修改最终都是调用这里才做到修改的。你可以直接使用 `loot.modifiers` 获取 LootModifierManager。

## 注册无条件战利品表修饰器

无条件战利品表修饰器，则代表它会影响所有战利品表。

```javascript
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.item.IngredientAny;

// loot.modifiers.registerUnconditional(name as string, modifier as ILootModifier)

// 将删除所有战利品表抽奖结果，也就是删除所有了生物、方块掉落物和地牢生成。
loot.modifiers.registerUnconditional("loot_nuke", CommonLootModifiers.clearLoot());

// 删除在所有战利品表内在热力系列模组的物品
// 再提一遍，这里包括了方块、实体掉落物，该脚本同时会使热力系列的方块均无掉落物！
loot.modifiers.registerUnconditional("remove_thermal_items", CommonLootModifiers.remove(
    IngredientAny.getInstance().onlyIf("thermal_only", (stack) => stack.owner == "thermal")
);
```

## 战利品表条件

// TODO

## 删除模组的战利品修饰器

// TODO
