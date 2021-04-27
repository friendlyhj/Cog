# 全局战利品修饰器

战利品表用于确定方块掉落、实体掉落等很多内容。

而全局战利品修饰器系统允许你修改战利品。在这里先声明一点，CrT 并不能直接修改战利品表，它只能添加删除全局战利品修饰符。后者为 Forge 提供的 API，可以在战利品表抽奖完成后，在生效之前再次修改战利品。

诚然，数据包可以直接修改战利品表，但作用极其有限，只能简单的根据功能有限的战利品表谓词添加物品或删除所有物品。而且是根据 json 进行修改，并不直观，格式又非常固定。而 CrT 我们有 ZenCode 这一编程语言，自然使用更加灵活高效。