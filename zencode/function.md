# 函数

函数用于计算结果也可以打包一系列操作。

## 基本格式

```javascript
public function 函数名(参数表) as 返回类型名 {
    [代码]
    return 函数返回值;
}
```

`return` 关键字将会把指定的值返回至函数的调用点上，执行后续操作。若函数处理时，碰到 return 后，程序将跳出函数，不再执行之后的函数内操作（跳出所有循环）。所以一般 `return` 会在函数代码的最下方。

如果你的函数不需要返回值，则将返回类型名设置为 `void`。且 `return` 关键字将不是必要的了。当然你可以用它来让程序提前跳出函数块。

`public` 用于设置你的函数为公共函数（实际上以 1.12 的说法就是全局函数）。你就可以在任何脚本上使用这个函数。你可以不加 `public` ，不过这样以后这个函数就只能在一个脚本上使用。不要尝试定义两个同名的公共函数，这会产生意料之外的问题。

## 参数默认值

现在你可以给函数的某个参数设置默认值，这样在调用函数时，可以省略这个参数。见例子三。

## 例子

### 将删合成与加合成打包起来

```javascript
public function tweakRecipeShapeless(out as IItemStack, shapeless as IIngredient[]) as void {
    craftingTable.removeRecipe(out);
    craftingTable.addShapeless(out.registryName, out, shapeless); 
    // registryName 为 IItemStack 的一个 Getter，返回物品的注册名，用于指定配方名
}
```

### 另一个加法

```javascript
public function add(x as int, y as int) as int {
    return x + y;
}
```

### 默认参数

```javascript
public function mutliply(x as int, y as int = 1) as int {
    return x * y;
}

println(mutliply(1, 5));
println(mutliply(1)); // 相当于 multiply(1, 1)
```

