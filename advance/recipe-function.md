# 配方函数

配方函数可以做到动态决定配方输出，根据配方实际输入来决定配方的输出。

可能由于一些安全性问题，1.16 的配方函数比起 1.12 的少了 ICraftingInfo。所以，配方函数现在只能用于根据配方输入动态决定配方输出了。

与 1.12 相比你不需要标记材料了，配方函数的第二个参数就是实际的全部配方输入。由于配方的种类不同，配方函数有三个类型。

* `RecipeFunctionSingle`（实际上这个在 CrT 里没用过）
* `RecipeFunctionArray`（目前只用于无序合成）
* `RecipeFunctionMatrix`（目前只用于有序、镜像合成）

他们的第一个参数和 1.12 一样，是配方的原始输出，返回值也应该是配方的实际输出（自然的，返回空气便是无法合成）。但是第二个参数是配方的实际输入（但不包括放在合成台的物品的具体数量），分别是 `IItemStack` `IItemStack[]` `IItemStack[][]`

> 配方事件现在已移除，请自行监听全局的 MCItemCraftedEvent

## 例子

### 继承原有 NBT 的升级配方

将金镐升级为钻石镐，保留原有 NBT。

```javascript
import crafttweaker.api.item.IItemStack;

craftingTable.addShaped("tag",<item:minecraft:diamond_pickaxe>,[
    [<tag:items:forge:gems/diamond>, <tag:items:forge:gems/diamond>, <tag:items:forge:gems/diamond>],
    [<item:minecraft:air>, <item:minecraft:golden_pickaxe>.anyDamage(), <item:minecraft:air>]],
    (out as IItemStack, ins as IItemStack[][]) => {
        return out.withTag(ins[1][1].tag);
    }
);
```

### 修复镐

每个钻石可以修复钻石镐 500 耐久。

```javascript
public function max(a as int, b as int) as int {
    return a > b ? a : b;
}

craftingTable.addShapeless("repair_pickaxe", <item:minecraft:diamond_pickaxe>, 
    [<item:minecraft:diamond_pickaxe>.onlyDamaged(), <tag:items:forge:gems/diamond>],
    (out as IItemStack, ins as IItemStack[]) => {
        return ins[0].withDamage(max(0, ins[0].damage - 500)); // 取最大值，为了防止负的耐久值
    }
);
```
