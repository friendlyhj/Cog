# 标签

标签（Tag）是 1.13 Mojang 新引入的概念，与 Forge 的矿物词典相似（且完全取代），且泛用性更高。物品标签与矿物词典类似，可让配方使用不同物品（如合成工作台可以用不同种类的木板）。方块标签用于指定方块的一些特殊属性。

更多信息见 [原版 Wiki](https://minecraft-zh.gamepedia.com/%E6%A0%87%E7%AD%BE)

## 导出

你可以用指令 `/ct dump tags` 导出游戏内所有 Tag ID。前者生成在 crafttweaker.log。这里有个问题，日志里打印的只是 ID，而非 zs 使用的尖括号调用。

## Import

如果你需要声明数组，请记得 `import`：

`import crafttweaker.api.tag.MCTag;`

## 尖括号调用

标签在 zs 的尖括号调用以 `tag` 开头： `<tag:forge:ingots/copper>` => 铜锭

## 修改

虽然标签有很多种，但 CrT 只支持物品、方块和实体标签。

标签的操作与旧版本的矿物词典非常相似。

但是需要注意的一点，如果你要创建一个新的标签，你必须用 `createXXXTag` 方法指定这个标签是用于什么的（物品、方块还是实体）。

* `createItemTag`
* `createBlockTag`
* `createEntityTypeTag`

```javascript
var wool = <tag:minecraft:wool>;
for item in wool.items {
    println(wool.commandString + " contains: " + item.displayName);
}


var out = <item:minecraft:string> * 4;
craftingTable.addShapeless("wool2string", out, [<tag:minecraft:wool>]); // 将 Tag 作为合成配方

<tag:minecraft:planks>.addItems([<item:minecraft:glass>]); // 移除物品
<tag:minecraft:wool>.removeItems([<item:minecraft:white_wool>]);

<tag:crafttweaker:ingots>.createItemTag(); // 将 <tag:crafttweaker:ingots> 指定物品标签
<tag:crafttweaker:ingots>.addItems([<item:minecraft:iron_ingot>, <item:minecraft:gold_ingot>]);

// 与上两行脚本等价
<tag:crafttweaker:ingots>.createItemTag().addItems([<item:minecraft:iron_ingot>, <item:minecraft:gold_ingot>]);

craftingTable.addShapeless("new_tag_test", <item:minecraft:diamond>, [<tag:crafttweaker:ingots>,<tag:crafttweaker:ingots>,<tag:crafttweaker:ingots>]);

<tag:minecraft:wither_immune>.addBlocks(<blockstate:minecraft:stone>); 
// 石头将抵御凋灵爆炸
// 方块状态尖括号调用不能直接指令导出
```
