# 配方替换

还记得 1.12 的 `recipes.replaceAllOccurence` 吗？这个方法允许你把所有配方的 A 物品替换为 B 物品。1.16 中，Replacer 提供了配方材料替换的功能，同时不像 1.12 只支持工作台，若模组提供了支持，Replacer 甚至可以修改别的模组的机器的配方！比起 1.12 一行解决问题，1.16 的 Replacer 比较复杂，但功能更强大！

## 导入

`import crafttweaker.api.recipe.Replacer;`

## 创建

首先你需要创建一个 Replacer。

* `Replacer.forEverything()` 创建一个会影响所有配方的 Replacer，但并非所有配方都允许替换材料，并不推荐使用。
* `Replacer.forTypes(managers as IRecipeManager...)` 创建一个会修改一个或多个 Recipe Types 下的 Replacer，如 `Replacer.forTypes(craftingTable)` 则是一个只会修改工作台配方的 Replacer。
* `Replacer.forMods(mods as string...)` 创建只修改一个或多个模组的配方的 Replacer，如 `Replacer.forMods("minecraft")` 是一个只会修改原版配方的 Replacer
* `Replacer.forOutput(output as IIngredient, managers as IRecipeManager...)` 创建只会修改输出指定物品的配方，managers 是 Recipe Types 白名单，如 `Replacer.forOutput(<tag:items:forge:blocks/copper>, craftingTable)` 只会修改各模组铜块（因为使用了 Tag）的工作台配方，managers 参数可省略，那就会尝试修改所有 Recipe Types 的指定物品的配方
* `Replacer.forRegexRecipes(regex as string)` 创建一个只会修改 ID 符合指定正则的 Replacer，如 `Replacer.forRegexRecipes("minecraft:wooden_.*")`
* `Replacer.forRegexTypes(regex as string)` 创建一个 ID 符合正则的 Recipe Type 以下的配方，如 `Replacer.forRegexTypes("minecraft:[a-z]*ing")`，虽然也只有工作台 ID 匹配这个正则
* `Replacer.forRecipes(recipes as WrapperRecipe[])` 创建只会修改特定配方的 Replacer，这里的参数是实打实的配方类，不是配方 ID，往往你需要 `IRecipeManager` 的 `getRecipeByName` 等方法获取。例子：`Replacer.forRecipes(craftingTable.getRecipeByName("minecraft:emerald_block"));`
* `Replacer.forCustomRecipeSet(myPredicate as BiPredicate<WrapperRecipe, IRecipeManager>);` 最高级的用法，以 WrapperRecipe 和 IRecipeManager 为参数，需要返回 boolean 的 lambda 表达式为参数，使用这个你可以完全指定符合什么条件的 Recipe Type 和什么条件的配方（WrapperRecipe）才会被修改。

> craftingTable 如果用 recipe type 尖括号表示，就是 `<recipetype:minecraft:crafting>`，但这有点长，CrT 内部已经把这个作为全局变量了。所以这里的 IRecipeManager，你能填 recipe type 尖括号，也能用前面原版配方修改使用的全局变量。craftingTable furnace 等等。

## 设置替换规则

创建完后，你需要设置替换规则，表示配方需要怎么改以及一些额外的排除信息。

### 排除

以下方法用于设置额外的排除规则，部分配方将会跳过处理。

* `excluding(managers as IRecipeManager...)` 哪些 Recipe Types 的配方将会被跳过处理。

`Replacer.forEverything().excluding(stoneCutter);` 修改所有配方，但切石机处理的配方将会被跳过。

* `excluding(recipes as MCResourceLocation[])` ID 为哪些的配方将会跳过处理。

`Replacer.forTypes(craftingTable).excluding(<resource:minecraft:comparator>, <resource:minecraft:boat>)` 修改所有工作台配方，跳过红石比较器和船的配方。请注意这里的参数是 MCResourceLocation 而不是 string，所以你填的格式是不一样的。MCResourceLocation 需要尖括号引用，前缀为 `resource`

* `excludingMods(mods as string...)` 哪些模组的配方将会被跳过。

`Replacer.forTypes(craftingTable).excludingMods("mekanism", "botania")` 修改所有工作台配方，除了 mek 和植魔的

### 替换

以下方法用于指定替换规则，哪些物品将会被替换成哪个。注意调用这些方法，只会使 Replacer 记录下这些规则，你最后还需要调用 `execute` 方法才会使 Replacer 开始工作！

> 这些方法返回 Replacer 本身，允许链性调用

`replace(from as IIngredient, to as IIngredient)`
`replaceFully(from as IIngredient, to as IIngredient)`

`replace` 方法是递归匹配的。只要配方的材料有这个物品，就会被替换。比如如果你使用 `replace(<item:minecraft:stone>, ...)`，这个配方如果使用 `<tag:item:minecraft:stones>` 这个石头物品标签，由于这个标签包含 `<item:minecraft:stone>`，也会被替换。如果你不想要这个行为，请使用 `replaceFully`

`replaceFully` 则是需要材料完全匹配，上面的例子如果你使用 `replaceFully` 则不会被替换。

## 执行

现在你设置好了替换目标、额外排除的内容和替换规则，你就可以最后调用 `execute` 无参方法使 Replacer 开始工作。**一定要调用这个方法，否则不会干活的。**

## 抑制警告

有很多配方不支持材料替换，别的模组的机器没做支持就先不提了。即使是普通工作台配方，也会有一些模组不讲武德注册了一些特殊的配方，CrT 无法处理它们，只能报一些警告告诉你部分配方无法被替换。你可以用 `excluding` 方法告诉 Replacer 要跳过这些配方，又或者直接调用 `suppressWarnings` 方法抑制这些警告。

## 尽可能少创建 Replacer

是的，你最好尽可能少的创建 Replacer。`execute` 方法将会遍历所有符合条件的配方。比如 `Replacer.forTypes(craftingTable)` 就会遍历所有工作台配方，而工作台配方有多少无需多言了。如果你创建了五个这样的 Replacer 就会遍历五次工作台配方，这会造成大量的性能浪费。你应该创建一个 Replacer，而设置多个替换规则。

## 例子

```kotlin
import crafttweaker.api.recipe.Replacer;

Replacer.forTypes(craftingTable) // 修改工作台配方
    .excludingMods("mekanism") // 排除 mek 配方
    .suppressWarnings() // 抑制警告
    .replace(<item:minecraft:gold_ingot>, <item:minecraft:stick>) // 替换规则 A：金锭替换成木棍
    .replace(<item:minecraft:diamond>, <item:minecraft:dirt>) // 替换规则 B：钻石替换成泥土
    .replace(<tag:items:forge:ingots/copper>, <item:minecraft:stone>) // 替换规则 C：铜锭替换成石头
    .execute(); // 最后记得调用这个方法开始执行替换
```
