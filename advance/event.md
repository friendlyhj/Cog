# 事件

事件是干预原版逻辑的一个有力的方式。首先要说明的是这个事件系统并不是原版自带的，这个事件系统是 Forge 修改原版代码后实现的，CraftTweaker 将事件系统交给 ZenCode 处理。注册事件的逻辑很简单，监听一个事件，当这个事件触发时，将执行哪些代码。在这里你就是完全把 ZenCode 当成编程语言了。有些事件可以取消，事件取消后可以使一些原版行为不会发生。

## Import

你需要导入事件管理器：

`import crafttweaker.api.events.CTEventManager`

## 注册事件

与 1.12 需要查找事件类和对应的事件注册方法相比，1.16 的事件注册相对简单一些。只需要查阅事件类就行，因为 ZenCode 支持泛型，注册事件的方法也就使用了泛型，那么只需要一个方法就够了。

`CTEventManager.register<T : MCEvent>(consumer as Consumer<T>);`

将你需要监听的事件类作为泛型，而参数则是以事件实例为参数，无返回值的 lambda 表达式，表示怎么处理这个事件。

## 例子

当玩家丢弃物品时，发送信息给玩家，内容是他扔掉了啥物品。

```java
import crafttweaker.api.events.CTEventManager; // 事件管理器
import crafttweaker.api.event.item.MCItemTossEvent; // 丢弃物品事件
import crafttweaker.api.util.text.MCTextComponent; // 给玩家发消息需要的类，你在修改 tooltip 的时候应该见到过了

CTEventManager.register<MCItemTossEvent>(event => { // 我们监听丢弃物品事件
    val item = event.entityItem.item;
    val player = event.player;
    player.sendMessage(MCTextComponent.createStringTextComponent("You tossed " + item.commandString + " !"));
});
```

## 服务端与客户端

如果你加载了上面的脚本，你会发现消息发了两次。这是由于 MC 是一个双端游戏：客户端和服务端。但很多游戏逻辑是双端耦合的。所以这个事件会在双端各触发一次，消息自然也发了两遍。然而只有服务端用来处理游戏主逻辑，存档也是保存在服务端的，而客户端只是用来进行渲染，键鼠输入的。我们使用 CraftTweaker 是为了干预原版游戏逻辑，管不着客户端的行为。所以我们的事件处理大多需要跳过客户端，即如果是客户端，则什么事都不干。

要区分客户端和服务端，你可以通过 `MCWorld` 的 `remote` getter，在服务端这个 getter 返回 `false`，客户端为 `true`。事件中无论如何，你都能找到 `MCWorld`，可能事件直接有 `world` getter，也可以通过实体获取。上面的例子则可以这么干。

> 此外，如果在服务端，你可以用 `MCWorld` 的 `asServerWorld` 方法，将其转换成可用方法更多的 `MCServerWorld`

```java
CTEventManager.register<MCItemTossEvent>(event => {
    val item = event.entityItem.item;
    val player = event.player;
    val world = player.world; // 获取玩家所在世界
    if (world.remote) { // 如果是客户端
        return; // 使用 return 跳出函数，不执行下面的代码
    }
    player.sendMessage(MCTextComponent.createStringTextComponent("You tossed " + item.commandString + " !"));
});
```
