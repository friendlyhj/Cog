# 原版配方修改

## IIngredient

IIngredient ，材料接口，可以简单理解为既能是物品，也能是标签。

## 工作台配方

不是 1.12 的 `recipe.addShaped` 的了，而是 `craftingTable.addShaped` 了。

### 有序合成

` craftingTable.addShaped(recipeName, output, inputBox);`

* recipeName 是一个字符串，指定配方的名称（1.12 可省略，现在必须填！）
* output 是配方的输出，只能是物品，不能是标签
* inputBox 是材料的二维数组，指定配方输入（没必要填满 3 * 3 的）

```javascript
craftingTable.addShaped("recipe_test", <item:minecraft:dirt>, [
                        [<item:minecraft:diamond>], [<tag:minecraft:wool>]
]);
```

### 无序合成

` craftingTable.addShapeless(recipeName, output, inputBox);`

* inputBox 是材料的数组，只有一层中括号

```javascript
craftingTable.addShapeless("recipe_name", <item:minecraft:sand>, [<item:minecraft:diamond>, <tag:minecraft:planks>]
```

### 镜像合成

镜像合成是特殊的有序配方，它的输入可以水平翻转。

` craftingTable.addShapedMirrored(recipeName, output, inputBox);`

### 移除配方

| 基本格式                             | 作用                             |
| ------------------------------------ | -------------------------------- |
| `craftingTable.removeRecipe(output);` | 删除物品的合成                   |
| `craftingTable.removeByName(name);`   | 删除特定名称的配方               |
| `craftingTable.removeByModid(modid);` | 删除特定模组的全部配方           |
| `craftingTable.removeByRegex(modid);` | 删除名称匹配正则表达式的所有配方 |
| `craftingTable.removeAll();`          | 删除全部配方                     |
