# 通用模组修改

在模组配方修改时，我们需要一个 Recipe Type，而使用数据包修改时，我们从来没有担心过这个。这是因为根据 json 转换出的配方会根据 `IRecipe#getRecipeType` 方法来确定他应该加在哪个 Recipe Type 里。在 CrT 7.1.0.243 添加了 GenericRecipeManager，你只需要配方名和 json 就可以直接添加任何配方。

## 调用

使用 `recipes` 全局变量即可。

> 是的，在 1.12.2 及以下的版本，recipes 代表的是工作台配方。
>
> 在 1.12.2 之前，IRecipe 的概念只存在于工作台中，而在高版本，IRecipe 用在了各种机器，从原版的熔炉到星辉的祭坛都使用了它。
>
> recipes 从只代表工作台配方升格为游戏内的全部配方，而工作台配方则变成了 craftingTable（即工作台的英文）。
>
> 若要针对特定机器的配方，参考上章，用 Recipe Type 尖括号引用。

## 添加配方

`recipes.addJSONRecipe(name as String, data as IData);`

name 就是配方名，相当于你在数据包添加的配方的文件名。

data 就是配方的具体内容，就是你在数据包添加的配方的文件内容。

当然上章也说过，我们可以复用 CrT 类，它们会自动转换为 IData。（虽然不能很好的解决材料数量）

```less
recipes.addJSONRecipe("recipe_name", {
    type: "minecraft:smoking",
    ingredient: <item:minecraft:gold_ore>,
    result: <item:minecraft:cooked_porkchop>,
    experience: 0.35 as float,
    cookingtime: 100
 });
```

## 移除配方

与工作台配方一样，`craftingTable` 改成 `recipes` 即可。但注意它会影响游戏内所有 Recipe Type 的配方。

```less
// 将会删除魔力钢锭的全部配方，包括块分锭和粒合锭的工作台配方以及铁锭的魔力灌注配方。
recipes.removeRecipe(<item:botania:manasteel_ingot>);
```
