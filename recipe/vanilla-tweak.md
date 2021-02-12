# 原版配方修改

## IIngredient

IIngredient ，材料接口，可以简单理解为既能是物品，也能是标签。如果你看见了 IIngredient，则意味着这里既能填物品，也能填标签。

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
craftingTable.addShapeless("recipe_name", <item:minecraft:sand>, [<item:minecraft:diamond>, <tag:items:minecraft:planks>]
```

### 镜像合成

镜像合成是特殊的有序配方，它的输入可以水平翻转。

`craftingTable.addShapedMirrored(recipeName, output, inputBox);`

### 移除合成

| 基本格式 | 作用 |
| :--- | :--- |
| `craftingTable.removeRecipe(output);` | 删除特定物品的合成 |
| `craftingTable.removeByName(name);` | 删除特定名称的配方 |
| `craftingTable.removeByModid(modid);` | 删除特定模组的全部配方 |
| `craftingTable.removeByRegex(regex);` | 删除名称匹配正则表达式的所有配方 |
| `craftingTable.removeAll();` | 删除全部配方 |

## 熔炉

### 添加配方

```javascript
furnace.addRecipe(name, output, input, xp, cookTime);

furnace.addRecipe("wool2diamond", <item:diamond>, <tag:items:minecraft:wool>, 1.0, 0);
```

* name: 字符串，配方名
* output: 物品，配方输出
* input: IIngredient，配方输入
* xp: 双精度浮点数，该配方产生的经验值
* cookTime: 整数，配方所需时间（如果填 0 ，则取缺省值 200）

### 移除配方

| 基本格式 | 作用 |
| :--- | :--- |
| `furance.removeRecipe(output, input);` | 删除一个特定配方（input 为 材料） |
| `furance.removeRecipe(output);` | 删除特定物品的配方 |
| `furance.removeByName(name);` | 删除特定名称的配方 |
| `furance.removeByModid(modid);` | 删除特定模组的全部配方 |
| `furance.removeByRegex(regex);` | 删除名称匹配正则表达式的所有配方 |
| `furance.removeAll();` | 删除全部配方 |

### 熔炉变种

烟熏炉把 `furance` 换成 `smoker`，高炉换成 `blastFurnace` 即可。

### 熔炉燃料

`item.burnTime = value;`

`<item:minecraft:diamond>.burnTime = 10000;`

将钻石的燃料时间改为 10000，如果设置为 0 则为删除。

## 营火

### 添加配方

```javascript
campfire.addRecipe(name, output, input, xp, cookTime);

campfire.addRecipe("wool2diamond", <item:diamond>, <tag:items:minecraft:wool>, 1.0, 0);
```

* name: 字符串，配方名
* output: 物品，配方输出
* input: IIngredient，配方输入
* xp: 双精度浮点数，该配方产生的经验值
* cookTime: 整数，配方所需时间（如果填 0 ，则取缺省值 100）

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
val a = <item:minecraft:iron_ingot>.withTag({display: {Lore: ["233"]}});

craftingTable.addShapeless("ttt", <item:minecraft:sand>, [a, a, a]);
```

## IngredientList

你可以将两个材料用 `|` 连接起来，这样这个位置就可以使用两个材料的任意一个。

```javascript
craftingTable.addShaped("recipe_test_3", <item:minecraft:gold_ingot> * 2, [
    [<item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>, <item:minecraft:iron_ingot>],
    [<item:minecraft:iron_ingot>, <item:minecraft:air>, <item:minecraft:iron_ingot>],
    [<tag:items:minecraft:wool> | <tag:items:minecraft:planks>, <item:minecraft:air>, <tag:items:minecraft:wool> | <tag:items:minecraft:planks>]
]);
```

这样第七个和第九个格子可以使用任意羊毛和木板。
