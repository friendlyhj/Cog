# 创造标签

CoT 还允许你创建一个创造标签。

```csharp
#loader contenttweaker
import crafttweaker.api.item.ItemGroup;
import crafttweaker.api.BracketHandlers;

// 通过 ItemGroup 的拓展方法来创建一个新的创造标签
// 第一个参数是名称，第二个参数是 lambda 表达式，用于设定图标
// 这个标签不可用尖括号引用获取，所以你必须将它保存在变量里后续使用
var tab = ItemGroup.create("contenttweaker", () => <item:minecraft:dragon_egg>);

// 由于尖括号引用会在脚本编译时检查是否存在
// 但创造标签建立时，物品还未注册
// 所以对于 CoT 物品，使用尖括号获取图标物品会报错
// 若你要用 CoT 物品来作为标签，你必须使用 BracketHandlers 类来获取物品
var tab2 = ItemGroup.create("contenttweaker_2", () => BracketHandlers.getItem("contenttweaker:test"));

// 简单的创建物品，并设定创造标签
new ItemBuilder()
    .withItemGroup(tab2)
    .build("test");
```

## lang key

本地化 key 为 `itemGroup.创造标签名`
