# Tooltip

Tooltip 是用于给物品添加除了物品名之外更多的信息。不只是模组上的一些物品，原版的附魔书、旗帜也都用了 tooltip。

## 添加 Tooltip

```javascript
// item.addTooltip(MCTextComponent content)
// 添加 tooltip
// 如果要多行，则多写几遍
// item.addShiftTooltip(MCTextComponent content, @ZenCodeType.Optional MCTextComponent showMessage)
// 按住 shift 才能看见的 tooltip，第二个参数可省略，为不按 shift 显示的 tooltip
// 这个 item 只能是物品 IItemStack

// MCTextComponent 为 Minecraft 的富文本格式。
// 在这里我们可以把 string 直接填进去，会自动转换成无特殊格式的 MCTextComponent

<item:minecraft:iron_ingot>.addTooltip("This is Iron");

<item:minecraft:gold_ingot>.addShiftTooltip("This is gold", "Hold SHIFT for more info");
```

## MCTextComponent

MCTextComponent 是富文本。所以我们可以指定他的格式。1.16 CrT 取消了 Formatter，所以我们只能通过以下方式搞。可能有一些麻烦。但是这也和内部 Java 本应该写法也基本统一了。

```javascript
import crafttweaker.api.util.text.MCTextComponent;
import crafttweaker.api.util.text.MCStyle;

// 给铁锭加上红色粗体的 tooltip
<item:minecraft:iron_ingot>.addTooltip(("This is Iron" as MCTextComponent).setStyle(new MCStyle().setColor(<formatting:red>).setBold(true)));
```

此外，这个类也进行了操作符重载。我们可以用 `+` `~` `<<` 来拼接两个 MCTextComponent。

## 清除 Tooltip

使用 `clearTooltip` 删除原有的所有 tooltip。

```javascript
<item:minecraft:enchanted_book>.clearTooltip();
```

## 动态添加 Tooltip

附魔书的 tooltip 不是固定的。说明他是动态的。在 CrT 也可以使用。

这里出现的 ITooltipFunction (`crafttweaker.api.item.tooltip.ITooltipFunction`) 是本篇教程你第一个看见的 lambda 表达式了。有三个参数：

* stack as `IItemStack`：具体的物品
* tooltip as `List<MCTextComponent>`：已经添加好的 tooltip，往其添加元素即可添加 tooltip。
* advance as `bool`：是否为高级 tooltip。只用于地图和 F3 + H。

```javascript
// item.modifyTooltip(ITooltipFunction tooltipFunction);

// 给成书添加 tooltip，内容为其的 NBT

<item:minecraft:written_book>.modifyTooltip((stack, tooltip, advance) => {
    if (stack.hasTag) {
        tooltip.add(stack.tag.asString());
    }
});
```
