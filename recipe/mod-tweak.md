# 模组配方修改

## Recipe Type

Recipe Type 也是 1.13 Mojang 引入的概念，是配方的种类，用于数据包添加配方的时候使用 `type` 字段即可指定添加配方的种类（是无序配方、有序配方、熔炉还是其他）。在 1.14 新加的烟熏炉、切石机等新的功能性方块中更为突出。受其影响，大部分模组的机器也改用 Recipe Type。Recipe Type 是给数据包使用的。模组也是用内置的数据包来加机器的配方。作为一个主改配方的模组，CraftTweaker 自然对 Recipe Type 进行了支持。也就是说，理论上数据包、CraftTweaker 能修改所有模组的机器配方，无需模组的另外支持。

## 导出

你可以用指令 `/ct dump recipeTypes` 或者 `/ct dumpBrackets` 导出游戏内所有物品 ID。前者生成在 crafttweaker.log 里，后者在 ct\_dumps/recipetype.txt

## 添加配方

使用 `addJSONRecipe` 方法：`recipetypeObj.addJSONRecipe(recipeName, json);`

* recipeName: 字符串，是配方名
* json: 用 JSON 描述这个配方的所有信息，与数据包的写法相同，不过不需要 `type` 字段，因为这个在前面的 recipetypeObj 已经说明了。

### JSON 怎么写

两个字，拆包。任何模组都会有其内置的数据包，你可以通过模组内置数据包（即 `data` 文件夹）里的 `recipes` （即存放配方的）路径下找到 `type` 字段的值和你要改的机器配方 ID 相同的 json 文件，里面的内容即为给你的完美例子。

### 例子

以植物魔法符文祭坛为例。可在植物魔法模组的 `data/botania/recipes/runic_altar/` 路径下找到所有符文祭坛的配方。随便打开一个文件，如 `air.json`：

```javascript
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
runicAltar.removeByName("botania:runic_altar/spring"); // 当然用变量也可以了
```

## 复用 CrT 类

CraftTweaker 的 `IIngredient` 与 `MCTag<MCItemDefinition>`（即物品标签）支持自动转换为 IData。这意味的在加 `addJSONRecipe` 中，我们可以复用这些类，运行时这些对象会转换为 IData，达到构建出我们需要的 JSON 配方信息的目的。

```javascript
<recipetype:botania:runic_altar>.addJSONRecipe("test", {
    "output": <item:minecraft:gold_ingot>,
    "mana": 12000,
    "ingredients": [
        <item:minecraft:iron_ingot>,
        <tag:items:minecraft:wool>
    ]
});
```

## 材料数量

Mojang 反序列化 IIngredient，也是 CrT 将 IIngredient 转换成 IData 时，会丢失数量信息。事实上，Ingredient 是没有数量信息的。CraftTweaker 提供了 IngredientWithAmount 额外补充了一个数量信息，但也是由于以上原因，CrT 在把它转换成 IData 时也不会保留数量。原版的配方输入是没有数量的。但模组有，所以模组读取配方 JSON 信息时，会根据他们各自的方式来获取材料的数量，这是不确定的。在写完整的 JSON 配方时，按照模组数据包的例子不会出问题。但是如果你想要复用 CrT 类时，就会出问题。你可以自定义一个函数，将物品数量按照你要添加模组的读取格式重新加回去。

## Forge 对数据包的拓展

Forge 添加了 NBTIngredient 与 CompoundIngredient，这允许我们可以通过数据包来添加带有 NBT 的物品作为输入与输出以及做到 1.12 CrT IngredientOr 的效果。CrT 对应类同样支持这个。

```javascript
val a = <item:minecraft:iron_ingot>.withTag({display: {Lore: ["123" as string]}});
val b = <item:minecraft:apple>;

<recipetype:botania:runic_altar>.addJSONRecipe("test", {
  "output": <item:minecraft:gold_ingot>,
    "mana": 12000,
    "ingredients": [
        (a | b),
        <tag:items:minecraft:wool>
    ]
});
```

## 额外设置 Recipe Serializer

Recipe Serializer 用于将一个 JSON 转换成游戏里可用的配方。原版按照 JSON 的 type 字段来决定使用哪一个 Recipe Serializer。CrT 加 JSON 配方，会默认使用与 Recipe Type ID 相同的 Recipe Serializer。一般这样不会出现问题，所以我们也一般会省略 type 字段。但是有些模组的 Recipe Type 与 Recipe Serializer 的 ID 不同。这个时候我们依旧需要指定 Recipe Serializer Type。如星辉魔法的各个祭坛的配方添加。

> 摘自 <https://github.com/HellFirePvP/AstralSorcery/issues/1715#issuecomment-778901867>

```javascript
<recipetype:astralsorcery:simple_altar>.addJSONRecipe("stone2marbletest", {
    "type": "astralsorcery:altar",
    "altar_type": 0,
    "duration": 5,
    "starlight": 1,
    "pattern": [
        "_____",
        "_____",
        "__A__",
        "_____",
        "_____"
    ],
    "key": {
        "A": {
            "tag": "forge:stone"
        }
    },
    "output": [{
        "item": "astralsorcery:marble_raw",
        "count": 1
    }],
    "effects": [
        "astralsorcery:built_in_effect_discovery_central_beam"
    ]
});
```

## 模组的主动支持

有些模组比较友好，主动提供了 CrT 支持。这种模组的 Recipe Type 有 addRecipe 之类的方法。这种你便可以像和工作台一样，和 1.12 一样用一行填空题解决问题了。这个不同模组机器格式不同，自行看 CrT wiki 或模组文档了。

其实本章用的符文祭坛便有模组主动支持的，但是刚开始写这页时还没有支持，不过也一直放着作为例子吧。总有些模组不会提供支持。
