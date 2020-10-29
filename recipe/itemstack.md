# 物品

## 导出

你可以用指令 `/ct dump items` 或者 `/ct dumpdumpBrackets` 导出游戏内所有物品 ID。前者生成在 crafttweaker.log 里，后者在 ct\_dumps/item.txt

## Import

如果你需要声明数组，请记得 `import`：

`import crafttweaker.api.item.IItemStack;`

## Nonnull

与以前的版本不同，IItemStack 现在是 Nonnull 的，你不能把它设置为 null。也就是说，添加合成时空的位置以前填 `null`，现在得写成 `<item:minecraft:air>`。

## 不可省略的 item 尖括号调用

在 1.12 中，你可以用 `<minecraft:iron_ingot>` 这样表示一个物品。实际上你省略了最前面的 `item:`。而现在这个不可以省略了。你必须打成 `<item:minecraft:iron_ingot>`

