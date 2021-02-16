# 标签

标签（Tag）是 1.13 Mojang 新引入的概念，与 Forge 的矿物词典相似（且完全取代），且泛用性更高。物品标签与矿物词典类似，可让配方使用不同物品（如合成工作台可以用不同种类的木板）。方块标签用于指定方块的一些特殊属性。

更多信息见 [原版 Wiki](https://minecraft-zh.gamepedia.com/%E6%A0%87%E7%AD%BE)

## 导出

你可以用指令 `/ct dump tags` 导出游戏内所有 Tag ID。

<!-- ## Import

如果你需要声明数组，请记得 `import`：

`import crafttweaker.api.tag.MCTag;` -->

## 尖括号调用

标签在 zs 的尖括号调用以 `tag` 开头，此外需要再指定其的类型 `<tag:items:forge:ingots/copper>` => 铜锭。即其格式为 `<tag:type:namespace:path>`

标签类型有下面几个：

* items 物品标签，作用于旧版本 Forge 的矿物词典类似
* fluids 流体标签，默认有 `<tag:fluids:minecraft:lava>` 和 `<tag:fluids:minecraft:water>`，用于指定流体的行为
* blocks 方块标签，可以指定方块的一些行为
* entity_types 实体标签

## 修改

标签的操作与旧版本的矿物词典非常相似。不过标签名都是和物品 ID 一样，有命名空间的。

```javascript
var wool = <tag:items:minecraft:wool>;
for item in wool.elements {
    println(wool.commandString + " contains: " + item.commandString);
}


var out = <item:minecraft:string> * 4;
craftingTable.addShapeless("wool2string", out, [<tag:items:minecraft:wool>]); // 将 Tag 作为合成配方

// 添加与移除物品
<tag:items:minecraft:planks>.add(<item:minecraft:glass>);
<tag:items:minecraft:wool>.remove(<item:minecraft:white_wool>);

// 添加标签
<tag:items:crafttweaker:ingots>.add(<item:minecraft:iron_ingot>, <item:minecraft:gold_ingot>);

craftingTable.addShapeless("new_tag_test", <item:minecraft:diamond>, [<tag:items:crafttweaker:ingots>,<tag:items:crafttweaker:ingots>,<tag:items:crafttweaker:ingots>]);

<tag:blocks:minecraft:wither_immune>.add(<blockstate:minecraft:stone>); 
// 石头将抵御凋灵爆炸
```
