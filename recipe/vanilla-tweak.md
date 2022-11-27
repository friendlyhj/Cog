# 原版配方修改

## IIngredient

IIngredient，材料，是 Mojang 的标准化的在配方中用于匹配物品的东西。

标签虽是高版本矿物系统的替代品，但由于标签的作用更大一点，除了用于类似矿辞功能的物品标签，还有方块标签等等。只有物品标签才能够转换到 IIngredient 中。一般情况下，物品标签可以自动转换到 IIngredient 中。但如有意外情况，可以手动用 `asIIngredient` 方法进行转换。

如果你需要声明数组，你需要 `import` ： `import crafttweaker.api.item.IIngredient`

## 工作台配方

不是 1.12 的 `recipes.addShaped` 的了，而是 `craftingTable.addShaped` 了。

### 有序合成

`craftingTable.addShaped(recipeName, output, inputBox);`

* recipeName 是一个字符串，指定配方的名称（1.12 可省略，现在必须填！）
* output 是配方的输出，只能是物品，不能是标签
* inputBox 是 IIngredient 的二维数组，指定配方输入（没必要填满 3 \* 3 的）

```javascript
craftingTable.addShaped("recipe_test", <item:minecraft:dirt>, [
                        [<item:minecraft:diamond>], [<tag:items:minecraft:wool>]
]);
craftingTable.addShaped("recipe_test_2", <item:minecraft:gold_ingot>, [
    [<item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>],
    [<item:minecraft:iron_ingot>, <item:minecraft:air>, <item:minecraft:iron_ingot>],
    [<tag:items:minecraft:wool>, <item:minecraft:air>, <tag:items:minecraft:wool>]
]);
```

### 无序合成

`craftingTable.addShapeless(recipeName, output, inputBox);`

* inputBox 是材料的数组，只有一层中括号

```javascript
craftingTable.addShapeless("recipe_name", <item:minecraft:sand>, [<item:minecraft:diamond>, <tag:items:minecraft:planks>]);
```

### 镜像合成

镜像合成是特殊的有序配方，它的输入可以水平翻转。

`craftingTable.addShapedMirrored(recipeName, output, inputBox);`

### 移除合成

| 基本格式 | 作用 |
| :--- | :--- |
| `craftingTable.removeRecipe(output);` | 删除特定物品的合成 |
| `craftingTable.removeByName(name);` | 删除特定名称的配方 |
| `craftingTable.removeRecipeByInput(input);` | 删除含有特定物品的全部配方 |
| `craftingTable.removeByModid(modid);` | 删除特定模组的全部配方 |
| `craftingTable.removeByRegex(regex);` | 删除名称匹配正则表达式的所有配方 |
| `craftingTable.removeAll();` | 删除全部配方 |

```javascript
craftingTable.removeRecipe(<item:minecraft:iron_ingot>); // 删除所有（成品为）铁锭的配方
craftingTable.removeByName("minecraft:crafting_table"); // 删除 ID 为 minecraft:crafting_table 的配方
craftingTable.removeRecipeByInput(<item:minecraft:iron_ingot>); // 删除所有铁锭参与的配方
craftingTable.removeByMod("thermal"); // 删除所有热力系列的配方
craftingTable.removeByRegex("minecraft:.*"); // 删除所有 ID 匹配指定正则的配方
```

## 熔炉

### 添加配方

```javascript
furnace.addRecipe(name, output, input, xp, cookTime);

furnace.addRecipe("wool2diamond", <item:diamond>, <tag:items:minecraft:wool>, 1.0, 200);
```

* name: 字符串，配方名
* output: 物品，配方输出
* input: IIngredient，配方输入
* xp: 双精度浮点数，该配方产生的经验值
* cookTime: 整数，配方所需时间（Tip: 原版配方均为 200）

### 移除配方

| 基本格式 | 作用 |
| :--- | :--- |
| `furnace.removeRecipe(output, input);` | 删除一个特定配方（input 为 材料） |
| `furnace.removeRecipe(output);` | 删除特定物品的配方 |
| `furnace.removeByName(name);` | 删除特定名称的配方 |
| `furnace.removeByModid(modid);` | 删除特定模组的全部配方 |
| `furnace.removeByRegex(regex);` | 删除名称匹配正则表达式的所有配方 |
| `furnace.removeAll();` | 删除全部配方 |

### 熔炉变种

烟熏炉把 `furnace` 换成 `smoker`，高炉换成 `blastFurnace` 即可。

### 熔炉燃料

`item.burnTime = value;`

`<item:minecraft:diamond>.burnTime = 10000;`

将钻石的燃料时间改为 10000，如果设置为 0 则为删除。

## 营火

### 添加配方

```javascript
campfire.addRecipe(name, output, input, xp, cookTime);

campfire.addRecipe("wool2diamond", <item:minecraft:diamond>, <tag:items:minecraft:wool>, 1.0, 100);
```

* name: 字符串，配方名
* output: 物品，配方输出
* input: IIngredient，配方输入
* xp: 双精度浮点数，该配方产生的经验值
* cookTime: 整数，配方所需时间（Tip: 原版配方均为 100）

### 移除配方

与熔炉基本相同，把 `furance` 改成 `campfire` 即可。

## 切石机

### 添加配方

```javascript
stoneCutter.addRecipe(recipeName, output, input);

stoneCutter.addRecipe("recipe_name", <item:minecraft:grass>, <tag:items:minecraft:wool>);
```

* name: 字符串，配方名
* output: 物品，配方输出
* input: IIngredient，配方输入

### 移除配方

与工作台基本相同，把 `craftingTable` 改成 `stoneCutter` 即可。

## NBT

与 1.12 相同，你依旧可以用 `withTag` 来标记物品的 NBT，并将其作为配方的输入输出。

```javascript
val a = <item:minecraft:iron_ingot>.withTag({Display: {Lore: ["233"]}});

craftingTable.addShapeless("ttt", <item:minecraft:sand>, [a, a, a]);
```

## IngredientList

你可以将两个材料用 `|` 连接起来，这样这个位置就可以使用两个材料的任意一个。

在这里我们用了 `asIIngredient` 将标签转换为了材料。

```javascript
var woolAndPlank = <tag:items:minecraft:wool>.asIIngredient() | <tag:items:minecraft:planks>.asIIngredient();

craftingTable.addShaped("recipe_test_3", <item:minecraft:gold_ingot> * 2, [
    [<item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>],
    [<item:minecraft:iron_ingot>, <item:minecraft:air>, <item:minecraft:iron_ingot>],
    [woolAndPlank, <item:minecraft:air>, woolAndPlank]
]);
```

这样第七个和第九个格子可以使用任意羊毛和木板。
