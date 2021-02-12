# List

List 为 ZenCode Stdlib 引入的一个重要的集合类。可以存储多个对象。List 本身也是对象，你可以发现他也作为了很多方法的参数或返回值。你可以简单理解为「可变长数组」。

## Import

要想使用它，你必须先将其导入：

`import stdlib.List;`

## 声明

你有两种方法声明一个 List。

```javascript
// 必要导入略
// 创建一个空的 List
val emptyItems as List<IItemStack> = new List<IItemStack>();

// 将数组转换为 List
val items as List<IItemStack> = [<item:minecraft:iron_ingot>];
```

其中尖括号内的为类型参数（泛型），用于指定这个 List 将存放哪个类型的对象。

## 遍历与访问

与数组完全相同。包括遍历、存取修改数据、获取元素个数、判断元素是否在 List 内。

## 可用方法

除了数组的东西以外，List 类具有以下方法。

以下参数类型若为 T，则代表该 List 的参数类型。

| 名称 | 参数表 | 返回值 | 作用 | 例子 |
| --- | --- | --- | --- | ---- |
| add  | T     | void | 在 List 的末尾添加一个元素 | `list.add(<item:minecraft:iron_ingot>)` |
| insert | usize, T | void | 在指定索引位置插入一个元素 | `list.insert(2, <item:minecraft:iron_ingot>);` |
| remove | usize | T | 删除 List 位于指定索引的元素，返回删除的元素 | `list.remove(2);` |
| indexOf | T | usize | 返回一个元素在 List 的最先出现的索引位置，如果 List 不存在该元素，则返回 -1 | `list.indexOf(<item:minecraft:iron_ingot>)` |
| lastIndexOf | T | usize | 返回一个元素在 List 的最后出现的索引位置，如果 List 不存在该元素，则返回 -1 | `list.lastIndexOf(<item:minecraft:iron_ingot>)` |
