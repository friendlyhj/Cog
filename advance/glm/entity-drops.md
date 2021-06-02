# 实体掉落修改

实体掉落是非常经典的战利品表的运用。

## MCEntityType

MCEntityType 代表实体的类型（猪、羊、苦力怕...），可用 `/ct dump entityTypes` 来获取游戏内所有 EntityType。

如需导入，`import crafttweaker.api.entity.MCEntityType;`

## 给实体战利品表添加修饰器

```javascript
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.loot.modifiers.ILootModifier;

// entityType.addLootModifier(name as string, modifier as ILootModifier) // 给实体添加战利品表修饰器
// entityType.addPlayerOnlyLootModifier(name as string, modifier as ILootModifier) // 给实体添加只有玩家杀死后才会生效的修饰器

// 猪将改掉钻石
<entitytype:minecraft:pig>.addLootModifier("add_diamond", (loots, currentContext) => [<item:minecraft:diamond>]);

// 苦力怕在被玩家杀死后改掉金锭
<entitytype:minecraft:creeper>.addPlayerOnlyLootModifier("creeper_add_gold", (loots, currentContext) => [<item:minecraft:gold_ingot>]);
```

## 实用方法

CrT 还有更多实用方法来修改掉落物

```javascript
// entityType.addDrop(uniqueId as string, stack as IItemStack)
// entityType.addDrops(uniqueId as string, stacks as IItemStack[])
// 额外添加掉落物
<entitytype:minecraft:sheep>.addDrop("add_apple", <item:minecraft:apple>);

// entityType.addPlayerOnlyDrop(uniqueId as string, stacks as IItemStack[])
// entityType.addPlayerOnlyDrops(uniqueId as string, stacks as IItemStack[])
// 只有玩家杀死后才额外添加掉落物
<entitytype:minecraft:sheep>.addPlayerOnlyDrops("add_items", [<item:minecraft:book>, <item:minecraft:stone>]);

// entityType.addWeaponAndPlayerOnlyLootModifier(name as string, weapon as IItemStack, modifier as ILootModifier)
// entityType.addWeaponAndPlayerOnlyLootModifier(name as string, weapon as IItemStack, matchDamage as boolean, modifier as ILootModifier)
// entityType.addWeaponAndPlayerOnlyLootModifier(name as string, weapon as IItemStack, matchDamage as boolean, matchNbt as boolean, modifier as ILootModifier)
// entityType.addWeaponOnlyLootModifier(name as string, weapon as IItemStack, modifier as ILootModifier)
// entityType.addWeaponOnlyLootModifier(name as string, weapon as IItemStack, matchDamage as boolean, modifier as ILootModifier)
// entityType.addWeaponOnlyLootModifier(name as string, weapon as IItemStack, matchDamage as boolean, matchNbt as boolean, modifier as ILootModifier)

// 只有(玩家)使用特定武器杀死后才会使用这个战利品表修饰器，第一个方法不匹配耐久、数量、NBT，你可以使用后面两个来设定匹配
<entitytype:minecraft:sheep>.addWeaponOnlyLootModifier("fu", <item:minecraft:diamond_axe>, true, (loots, currentContext) => [<item:minecraft:diamond>]);
```
