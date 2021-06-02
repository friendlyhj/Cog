# 方块掉落修改

单单一个战利品表修饰器是不够的，我们需要把他注册进去，确定影响哪个战利品表。

在高版本，方块掉落也由战利品表控制，我们便可以修改它。

## MCBlock 和 MCBlockState

MCBlock 表示一类方块
而 MCBlockState（方块状态）则表示同一类方块在这个世界上的不同属性。如向上向下未伸出的活塞都是活塞（`<block:minecraft:piston>`)，而他们的方块状态不同（`<blockstate:minecraft:piston:facing=up,extended=false>` 和 `<blockstate:minecraft:piston:facing=down,extended=false>`)

你可以用 `/ct hand` 直接获取一个手上方块的 BEP（`<block:modid:name>`）。方块状态则需通过 F3 屏幕右部查看，然后自行转换为 CrT 的格式。

## 给方块战利品表添加修饰器

```javascript
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.loot.modifiers.ILootModifier;
// block.addLootModifier(name as string, modifier as ILootModifier) // 给方块加修饰器
// blockstate.addTargetedLootModifier(name as string, modifier as ILootModifier) // 给一个特定方块状态加修饰器

// 钻石矿石将掉落其本身
// 请注意两个尖括号引用，前面是钻石矿石方块，后面是钻石矿石物品
<block:minecraft:diamond_ore>.addLootModifier("diamond_ore", (loots, currentContext) => [<item:minecraft:diamond_ore>]);

// 上面有雪的草方块将掉落钻石
<blockstate:minecraft:grass_block:snowy=true>.addTargetedLootModifier("snowy_diamond", (loots, currentContext) => [<item:minecraft:diamond>]);

// 橡木将不掉落任何东西
<block:minecraft:oak_log>.addLootModifier("clear_oak_drop", CommonLootModifiers.clearLoot());
```

## 实用方法

CrT 还有更多实用方法来修改掉落物

```javascript
// block.addDrop(uniqueId as string, stack as IItemStack)
// block.addDrops(uniqueId as string, stacks as IItemStack[])
// blockstate.addTargetedDrop(uniqueId as string, stack as IItemStack)
// blockstate.addTargetedDrops(uniqueId as string, stacks as IItemStack[])
// 额外添加掉落物
<block:minecraft:crafting_table>.addDrop("one", <item:minecraft:diamond>);

// block.addToolDrop(uniqueId as string, tool as IItemStack, stack as IItemStack)
// block.addToolDrops(uniqueId as string, tool as IItemStack, stacks as IItemStack[])
// blockstate.addToolDrop(uniqueId as string, tool as IItemStack, stack as IItemStack)
// blockstate.addToolDrops(uniqueId as string, tool as IItemStack, stacks as IItemStack[])
// 使用什么工具后才会额外添加掉落物，工具匹配只匹配 ID，不匹配耐久、数量、NBT。
<block:minecraft:crafting_table>.addToolDrop("one", <item:minecraft:diamond_axe>, <item:minecraft:diamond>);

// block.addToolLootModifier(name as string, tool as IItemStack, modifier as ILootModifier)
// block.addToolLootModifier(name as string, tool as IItemStack, matchDamage as boolean, modifier as ILootModifier)
// block.addToolLootModifier(name as string, tool as IItemStack, matchDamage as boolean, matchNbt as boolean, modifier as ILootModifier)
// blockstate.addToolLootModifier(name as string, tool as IItemStack, modifier as ILootModifier)
// blockstate.addToolLootModifier(name as string, tool as IItemStack, matchDamage as boolean, modifier as ILootModifier)
// blockstate.addToolLootModifier(name as string, tool as IItemStack, matchDamage as boolean, matchNbt as boolean, modifier as ILootModifier)
// 使用什么工具后才会使用这个战利品表修饰器，第一个方法不匹配耐久、数量、NBT，你可以使用后面两个来设定匹配

// 只有使用满耐久的钻石斧破坏工作台才会把掉落物改成钻石
<block:minecraft:crafting_table>.addToolDrop("one", <item:minecraft:diamond_axe>, true, (loots, currentContext) => [<item:minecraft:diamond>]);
```
