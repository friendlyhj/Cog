# 指令

CraftTweaker 提供了以下指令：

本教程只讲常用的，其实游戏内输入 `/ct` 之后就有自动补全了。

| 指令 | 作用 |
| :--- | :--- |
| `/ct hand` | 打印你手中的物品的 ID 和拥有的 Tag，以 zs 的尖括号引用方式，同时将物品 ID 导入剪贴板 |
| `/ct log` | 打开 CraftTweaker 的日志文件 |
| `/ct scripts` | 打开 scripts 文件夹 |
| `/ct inventory` | 打印物品栏内的所有物品的 ID 至日志 |
| `/ct dump <type>` | 导出游戏内所有特定的对象的 ID 至日志（如物品、流体、buff……） |
| `/ct dumpBrackets` | 导出游戏内部分种类的所有对象的 ID 至 ct\_dumps 文件夹中 |
| `/ct syntax` | 检查脚本是否有语法错误（不过有了热重载后，不会有人再用了吧） |
| `/ct examples` | 将会生成示例脚本 |
| `/reload` | 热重载脚本（不是 `/ct reload` ！） |

> 关于 [战利品修饰器](/advance/glm/modifier-manager.md) 的示例脚本，其的效果是给每一次战利品表抽奖结果加一个钻石，所以当你生成了示例脚本后，每次方块破坏、生物掉落都会多一个钻石…… 这不是 CrT 的 bug，看完示例脚本后记得删掉。
