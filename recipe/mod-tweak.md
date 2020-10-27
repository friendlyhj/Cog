# 模组配方修改

## Recipe Type

Recipe Type 也是 1.13 Mojang 引入的概念，是配方的种类，用于数据包添加配方的时候使用 `type` 字段即可指定添加配方的种类（是无序配方、有序配方、熔炉还是其他）。在 1.14 新加的烟熏炉、切石机等新的功能性方块中更为突出。受其影响，大部分模组的机器也改用 Recipe Type。Recipe Type 是给数据包使用的。模组也是用内置的数据包来加机器的配方。作为一个主改配方的模组，CraftTweaker 自然对 Recipe Type 进行了支持。也就是说，理论上数据包、CraftTweaker 能修改所有模组的机器配方，无需模组的另外支持。

## 导出

你可以用指令 `/ct dump recipeTypes 或者 `/ct dumpdumpBrackets` 导出游戏内所有物品 ID。前者生成在 crafttweaker.log 里，后者在 ct_dumps/recipetype.txt

## 添加配方

使用 `addJSONRecipe` 方法：`recipetypeObj.addJSONRecipe(recipeName, json);`

* recipeName: 字符串，是配方名
* json: 用 JSON 描述这个配方的所有信息，与数据包的写法相同，不过不需要 `type` 字段，因为这个在前面的 recipetypeObj 已经说明了。

### JSON 怎么写

两个字，拆包。任何模组都会有其内置的数据包，你可以通过模组内置数据包（即 `data` 文件夹）里的 `recipes` （即存放配方的）路径下找到 `type` 字段的值和你要改的机器配方 ID 相同的 json 文件，里面的内容即为给你的完美例子。

### 例子

以植物魔法符文祭坛为例。可在植物魔法模组的 `data/botania/recipes/runic_altar/` 路径下找到所有符文祭坛的配方。随便打开一个文件，如 `air.json`：

```json
{
  "type": "botania:runic_altar",
  "output": {
    "item": "botania:rune_air",
    "count": 2
  },
  "mana": 5200,
  "ingredients": [
    {
      "tag": "forge:dusts/mana"
    },
    {
      "tag": "forge:ingots/manasteel"
    },
    {
      "tag": "minecraft:carpets"
    },
    {
      "item": "minecraft:feather"
    },
    {
      "item": "minecraft:string"
    }
  ]
}
```

在 CrT 中 `type` 字段不用写，根据这个例子，可以很快写出对应的 JSON。

```javascript
<recipetype:botania:runic_altar>.addJSONRecipe("test", {
    "output": {
        "item": "minecraft:gold_ingot"
    },
    "mana": 12000,
    "ingredients": [
        {
            "item": "minecraft:ender_pearl"
        },
        {
            "item": "minecraft:iron_ingot"
        }
    ]
});
```

## 移除配方

和原版工作台类似，把 `craftingTable` 换成对应 Recipe Type 的 ID 即可

```javascript
<recipetype:botania:runic_altar>.removeAll(); // 删除所有符文祭坛配方

val runicAltar = <recipetype:botania:runic_altar>;
runicAltar.removeByName("botania:spring"); // 当然用变量也可以了
```

## 不兼容 NBT

综上所述，对模组配方的兼容是通过数据包实现的。而众所周知，数据包配方不支持 NBT。所以就有一大遗憾，模组的配方添加将不再兼容 NBT，对于 NBT 有额外支持的 Recipe Type 例外。