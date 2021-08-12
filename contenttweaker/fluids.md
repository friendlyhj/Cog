# 流体

不用我多说，你肯定能猜到创建流体需要一个 `FluidBuilder`。

导入：`import mods.contenttweaker.fluid.FluidBuilder;`

## 创建 FluidBuilder

FluidBuilder 有两个构造函数：

```kotlin
// new FluidBuilder(isMolten as boolean, color as int)
new FluidBuilder(true, 0x66ccff);
// isMolten: 这个流体是不是类似熔融的，类似熔岩的
// color：流体颜色，ARGB 格式，但若你没有设置 alpha 值，会将 alpha 值设置为 0xff，（即 0x66ccff 会被处理为 0xff66ccff）
// 这样创建的流体将会使用内置材质并进行染色，一般而言，你用这个就行，也不必担心材质的问题。

// 若你需要使流体材质与水与熔岩完全不同，而非只是颜色的区分，则需要使用这个构造函数
// new FluidBuilder(isMolten as boolean, color as int, stillTexture as MCResourceLocation, flowTexture as MCResourceLocation)
// 额外设置静止和流动材质
// color 参数将只用于自动生成的流体桶，而不会对流体本身染色
new FluidBuilder(true, 0x66ccff, <resource:contenttweaker:fluid/liquid>, <resource:contenttweaker:fluid/liquid_flowing>);

```

## 设定属性

流体有一部分参数可用，但这是 Forge 给流体的一些参数，不会影响原版的游戏机制，只可能用于部分 mod 的计算。（比如温差发电机可能会读取流体的温度）

| 方法及参数 | 例子 | 描述 |
| ---- | ----- | ---- |
| `density(density as int)` | `.density(1400);` | 密度，默认值为 1000 |
| `gaseous()` | `.gaseous()` | 类似气体 |
| `luminosity(luminosity as int)` | `.luminosity(15);` | 亮度，默认值为 0 |
| `temperature(temperature as int)` | `.temperature(500);` | 温度，默认值为 300 |
| `viscosity(viscosity as int)` | `.viscosity(800);` | 亮度，默认值为 1000 |

## 注册流体

当你所有参数都设置完了，可以调用 `build(name as string)` 方法来注册物品，参数为这个方块的注册名，只允许包含阿拉伯数字、小写字母和下划线 `_`。

## lang key

CoT 创建流体后，还会创建装有该流体的桶物品，这两个都需要给语言文件设值。

流体的 lang key 为 `fluid.contenttweaker.流体名`
桶的 lang key 为 `item.contenttweaker.流体名_bucket`

## 标签

流体创建好了，但你会发现这个流体好像不存在一样，实体在这个流体好像什么事都没有。这是因为 mojang 把一些流体的行为交给了标签处理，你需要给你的流体添加原版的水或熔岩标签。

虽然标签的操作在前面已经说过了，但还是在这里重复一次。

```kotlin
#loader crafttweaker
// tag 操作需要放在 CrT 脚本

// 静止和流动的流体其实是两种流体，你都需要加上标签
<tag:fluids:minecraft:water>.add(<fluid:contenttweaker:generic_fluid>);
<tag:fluids:minecraft:water>.add(<fluid:contenttweaker:generic_fluid_flowing>)
```

标签产生的行为见：[https://minecraft.fandom.com/zh/wiki/%E6%A0%87%E7%AD%BE#.E6.B5.81.E4.BD.93]

你可以发现，加上水标签后，实体是会被这个流体推动了，但还有一堆其他特性，你可能需要斟酌一下要不要加上标签。
