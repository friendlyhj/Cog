# 战利品修饰器管理器

LootModifierManager 管理了所有的战利品表修饰器，前两章的修改最终都是调用这里才做到修改的。你可以直接使用 `loot.modifiers` 获取 LootModifierManager。从这里注册的战利品表修饰器是全局的，往往需要战利品条件来确定是否生效。

## 注册无条件战利品表修饰器

无条件战利品表修饰器，则代表它会影响所有战利品表。

```javascript
import crafttweaker.api.loot.modifiers.CommonLootModifiers;
import crafttweaker.api.item.IngredientAny;

// loot.modifiers.registerUnconditional(name as string, modifier as ILootModifier)

// 将删除所有战利品表抽奖结果，也就是删除所有了生物、方块掉落物和地牢生成。
loot.modifiers.registerUnconditional("loot_nuke", CommonLootModifiers.clearLoot());

// 删除在所有战利品表内在热力系列模组的物品
// 再提一遍，这里包括了方块、实体掉落物，该脚本同时会使热力系列的方块均无掉落物！
loot.modifiers.registerUnconditional("remove_thermal_items", CommonLootModifiers.remove(
    IngredientAny.getInstance().onlyIf("thermal_only", (stack) => stack.owner == "thermal")
);
```

## 注册带条件战利品表修饰器

你可以使用战利品条件来确定什么时候执行以下的战利品修饰器，可使用以下方法。

`loot.modifiers.register(name as string, conditions as ILootCondition[], modifier as ILootModifier)`

`loot.modifiers.register(name as string, builder as LootConditionBuilder, modifier as ILootModifier)`

### 战利品条件

ILootCondition 是 LootContext 为参数，boolean 为返回值的函数式接口，用于确定何时执行修饰器。上面的第一个方法的 ILootCondition 数组为「与」操作，即当所给所有的战利品条件均满足后才会执行。本教程不会对这个函数式接口再进行更多介绍，因为你更应该用第二个接受 LootConditionBuilder 参数的方法。

### 战利品条件构建器

ILootConditionTypeBuilder 用于构建单个 ILootCondition。CrT 提供了大量 ILootConditionTypeBuilder 的实现类，有用于检测某一类型的，有用于逻辑判断的 CrT 提供的特殊类型。

LootConditionBuilder 则用于连接多个 ILootConditionTypeBuilder，进而构建出所需的 `ILootCondition` 数组。

一般而言，我们需要创建一个 LootConditionBuilder，往其添加一个或多个 ILootConditionTypeBuilder，最后将其传入上文的 `register` 方法完成操作。

> 是不是觉得有些套娃？这采用了抽象工厂模式 (Abstract Factory Pattern)

#### 创建战利品条件构建器

你可以调用以下方法来创建 LootConditionBuilder

```javascript
import crafttweaker.api.loot.conditions.LootConditionBuilder;
import crafttweaker.api.loot.conditions.crafttweaker.And;
import crafttweaker.api.loot.conditions.crafttweaker.Or;

// LootConditionBuilder.create();
// 添加一个空的 LootConditionBuilder

// LootConditionBuilder.createForSingle<T extends ILootConditionTypeBuilder>(lender as Consumer<T>)
// 添加后并加入一个 ILootConditionTypeBuilder

// LootConditionBuilder.createInAnd(lender as Consumer<And>)
// 你可以在 lender 函数内可以添加多个 ILootConditionTypeBuilder，这些为与操作

// LootConditionBuilder.createInOr(lender as Consumer<Or>)
// 你可以在 lender 函数内可以添加多个 ILootConditionTypeBuilder，这些为或操作
```

#### 添加条件

在创建 LootConditionBuilder 后，你可以使用 `add` 方法来添加更多的 ILootConditionTypeBuilder。

该方法是一个泛型方法。

```javascript
// builder.add<T extends ILootConditionTypeBuilder>(lender as Consumer<T>) as LootConditionBuilder
```

尖括号内填所需的 ILootConditionalTypeBuilder 的类名，而括号内的 lender 则为以所给泛型的实例为参数，无返回值的 lambda 表达式，用于设定一些数值。

这个方法返回 LootConditionBuilder 本身，则允许你 *链性调用* 多个 `add` 方法，来添加多个 ILootConditionTypeBuilder。

### 例子

```javascript
import crafttweaker.api.loot.conditions.LootConditionBuilder;
import crafttweaker.api.loot.conditions.vanilla.LootTableId;
import crafttweaker.api.loot.conditions.vanilla.RandomChance;
import crafttweaker.api.loot.modifiers.CommonLootModifiers;

var lcb = LootConditionBuilder.create() // 创建一个空的
    // 添加一个 ILootConditionBuilder，为 LootTableId，
    // 用于限定作用于哪个战利品表，例子为沙漠神殿
    // 尖括号泛型为 LootTableId，则 lambda 表达式的参数为 LootTableId 对象
    // 等等，你说战利品表 ID 从哪看？输入 `/ct dump loot_tables` 可以查看游戏内所有战利品表 ID
    // 例子调用 LootTableId 类的 withTableId 方法设定这个条件指定的战利品表
    .add<LootTableId>(condition => {
        condition.withTableId(<resource:minecraft:chests/desert_pyramid>);
    })
    // 同理，再添加 ILootConditionBuilder，为 RandomChance
    // 这里的 lambda 表达式参数即为 RandomChance
    // 调用 RandomChance#withChance 方法，来设定生效几率
    .add<RandomChance>(condition => {
        condition.withChance(0.7);
    });

// 注册战利品修饰符，这个修饰符只有战利品表是沙漠神殿和 70% 几率生效，效果是添加一本书。
loot.modifiers.register("add_book", lcb, CommonLootModifiers.add(<item:minecraft:book>));
```

铁矿在下雨的时候将直接会掉落铁锭，并排除精准采集的情况。

```javascript
import crafttweaker.api.loot.conditions.LootConditionBuilder;
import crafttweaker.api.loot.conditions.vanilla.MatchTool;
import crafttweaker.api.loot.conditions.vanilla.WeatherCheck;
import crafttweaker.api.loot.conditions.crafttweaker.Not;
import crafttweaker.api.loot.conditions.vanilla.BlockStateProperty;
import crafttweaker.api.loot.modifiers.CommonLootModifiers;

loot.modifiers.register(
    "drop_iron_ingot",
    LootConditionBuilder.create()
        // 下雨
        .add<WeatherCheck>(condition => {
            condition.withRain();
        })
        // 指定只作用于原版铁矿石
        .add<BlockStateProperty>(condition => {
            // 这里填的是 MCBlock
            condition.withBlock(<block:minecraft:iron_ore>);
        })
        // 对下面的条件取非
        // 我们需要跳过精准采集，即为给下面匹配精准采集的条件取非
        .add<Not>(not => {
            not.withCondition<MatchTool>(condition => { // 匹配工具
                condition.withPredicate(predicate => { // 设置工具需要满足的条件
                    // 工具需要精准采集附魔
                    // 对于其他附魔，可以在后面的 lambda 表达式中设置所需等级等信息
                    predicate.withEnchantmentPredicate(<enchantment:minecraft:silk_touch>, it => {});
                });
            });
        }),
    // 将原有铁矿石掉落替换为铁锭
    CommonLootModifiers.replaceWith(<tag:items:forge:ores/iron>, <item:minecraft:iron_ingot>)
);
);
```

是的，就像上文例子那样，你基本不需要自己重新用 lambda 写一个 ILootCondition 或 ILootModifier。通过组合内置战利品条件和战利品修饰符，你就能满足 90% 的需求。

在官方文档的 `Vanilla/Api/Loot/Conditions/Vanilla` 和 `Vanilla/Api/Loot/Conditions/CraftTweaker` 章节你可以看见所有提供的 ILootConditionTypeBuilder。

## 删除模组的战利品修饰器

其他模组可能也会注册自己的战利品修饰器。比起添加，删除则相当简单，毕竟它只需要填入想要删除的 ID 即可...

你可以通过 `/ct dump loot_modifiers` 指令查看游戏内所有战利品修饰器。

```javascript
loot.modifiers.removeAll(); // 删除所有
loot.modifiers.removeByModId(modid as string); // 删除特定模组的
loot.modifiers.removeByName(name as string); // 删除指定 ID 的
loot.modifiers.removeByRegex(regex as string); // 删除所有 ID 匹配所给正则的
```
