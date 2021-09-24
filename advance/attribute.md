# 属性修改

玩家、怪物等的属性 (Attribute) 是指实体身上一系列描述生物的不同数值。这些数值有：

* 最大生命值
* 攻击伤害
* 攻击速度
* 移动速度
* ...

在 [原版 wiki](https://minecraft.fandom.com/zh/wiki/%E5%B1%9E%E6%80%A7) 中可以找到更详细的说明。当你手持剑时，使你的攻击伤害属性加上一定数值，这个叫做「属性修饰器」(Attribute Modifier) CrT 允许修改这一数值。

## 添加物品全局属性修饰器

与原版需要依赖 NBT 才能做到一把铁剑伤害不是 6 点而是其他数值不一样，CrT 允许直接修改物品的默认属性修饰器，世界里的所有该物品均会受到影响。

`IIngredient.addGlobalAttributeModifier(attribute as Attribute, name as string, value as double, operation as AttributeOperation, slotTypes as MCEquipmentSlotType[])`

`IIngredient.addGlobalAttributeModifier(attribute as Attribute, uuid as string, name as string, value as double, operation as AttributeOperation, slotTypes as MCEquipmentSlotType[])`

下面对这里的参数一一介绍：

* IIngredient: 所要修改的物品。但这里不是 IItemStack，而是 IIngredient，所以你还可以用标签，不过记得用 `asIIngredient` 方法把标签转换成材料才行。
* attribute: 要修改的属性，你可以用 `/ct dump attribute` 指令导出所有的可用属性
* uuid: 这个修饰器的唯一标识符，可省略，省略后将会指定一个随机的。你可以用这个参数来**覆盖**掉其他地方添加的修饰器，如工具默认的伤害（使用同一个 uuid）你可以用 `/ct hand attributes` 查看手上物品所有的属性修饰器信息。
* name: 将要添加的属性修饰器的名字
* value: 数值
* operation: AttributeOperation 修饰符的运算模式，具体意义见下
* slotTypes: MCEquipmentSlotType[] 这个物品位于哪里才会给实体属性进行修改。`/ct dump equipmentSlotType` 可以导出所有装备位置（主副手和四个护甲槽）

```kotlin
<tag:items:forge:ingots>.asIIngredient().addGlobalAttributeModifier(<attribute:minecraft:generic.attack_damage>, "Extra Power", 10, AttributeOperation.ADDITION, [<equipmentslottype:mainhand>]);
```

玩家主手手持任意金属锭，加 10 点攻击伤害。

## 删除物品全局属性修饰器

你还可以删除物品原有的属性修饰器。

```kotlin
// IItemStack.removeGlobalAttribute(attribute as Attribute, slotTypes as MCEquipmentSlotType[]) as void
// IItemStack.removeGlobalAttributeModifier(uuid as string, slotTypes as MCEquipmentSlotType[]) as void

<item:minecraft:dirt>.removeGlobalAttribute(<attribute:minecraft:generic.attack_damage>, [<equipmentslottype:chest>]);
<item:minecraft:dirt>.removeGlobalAttributeModifier("8c1b5535-9f79-448b-87ae-52d81480aaa3", [<equipmentslottype:chest>]);
```

## AttributeOperation

导入：
`import crafttweaker.api.entity.AttributeOperation;`

这是一个枚举，你可以使用：

* `AttributeOperation.ADDITION`
* `AttributeOperation.MULTIPLY_BASE`
* `AttributeOperation.MULTIPLY_TOTAL`

分别代表原版属性 NBT 的 Operation 的 0 1 2。具体的运算含义，也可见原版 wiki。

### 为特定物品添加属性修饰器

可是如果你不想要全局，只是要给特定物品添加。比如一个配方输出为有更多伤害的钻石剑。你需要使用 `withAttributeModifier` 方法。

```kotlin
// IItemStack.withAttributeModifier(attribute as Attribute, name as string, value as double, operation as AttributeOperation, slotTypes as MCEquipmentSlotType[], preserveDefaults as boolean) as IItemStack
// IItemStack.withAttributeModifier(attribute as Attribute, uuid as string, name as string, value as double, operation as AttributeOperation, slotTypes as MCEquipmentSlotType[], preserveDefaults as boolean) as IItemStack

<item:minecraft:dirt>.withAttributeModifier(<attribute:minecraft:generic.attack_damage>, "8c1b5535-9f79-448b-87ae-52d81480aaa3", "Extra Power", 10, AttributeOperation.ADDITION, [<equipmentslottype:mainhand>], true);
```

这个方法将会返回一个新的有指定的物品修饰器的物品，你可以将其用于配方输出等其他地方。参数意义与前面的相似，但还有一个 `preserveDefaults` 参数，用于指定是否保留默认的属性修饰器。比如如果给一个钻石胸甲修改护甲值，如果参数为 `false`，护甲韧性这个默认的属性修饰器不会保留，你只会得到一件只有护甲值的钻石胸甲，填 `true` 则会保留。
