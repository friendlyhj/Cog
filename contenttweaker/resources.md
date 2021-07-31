# 资源文件

有一部分内容是必须要用数据包和资源包实现的，比如物品方块的材质、语言文件，方块的掉落战利品表设置。与 1.12 的 Resource Loader 相同，你需要一个能加载外挂资源包的模组：

CoT 支持以下资源包加载模组：

* [Open Loader](https://www.curseforge.com/minecraft/mc-mods/open-loader)
* [The Loader](https://www.curseforge.com/minecraft/mc-mods/open-loader)
* [Global Data- & Resourcepacks](https://www.curseforge.com/minecraft/mc-mods/drp-global-datapack)

你需要安装这三个模组中其中一个（多装是没有意义的），当 CoT 检测到这三个模组有一个安装上后，会在外挂资源包里生成默认的文件。

* 默认材质，CoT 会自动生成一个白底红叉图片作为材质
* 默认模型和 blockstate json 映射
* 默认战利品表（方块破坏和爆炸时掉落其本身）

你可以在稍后修改这些文件。

## 语言文件

CoT 不会自动生成语言文件，需要你自行添加。一般而言，你需要一份英文和中文的语言文件。

创建 `<资源包路径>/assets/contenttweaker/lang/en_us.json` 和 `<资源包路径>/assets/contenttweaker/lang/zh_cn.json`。

如果你使用 OpenLoader，资源包路径为 `.minecraft/openloader/resources/contenttweaker`。

语言文件是一个键-值映射，键在这里会被叫做非本地化名、lang key、Translation key。键的内容是 `<block|item|fluid>.contenttweaker.<名字>` 语言文件是 json 格式，json 的格式在此不多赘述。

```json
{
  "block.contenttweaker.generic_block": "Generic Block",
  "item.contenttweaker.generic_item": "Generic Item",
  "item.contenttweaker.generic_item_2": "Generic Item the 2nd",
  "item.contenttweaker.generic_item_3": "Generic Item the charmed one"
}
```
