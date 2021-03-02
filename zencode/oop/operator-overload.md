# 运算符重载

让我们思考一个问题，IItemStack 如何表示数量的。`<item:minecraft:iron_ingot> * 3`。没错，用的是乘号。但是乘号按理只是用来做乘法的，它怎么在这里能用于表示数量呢？这便用的是「运算符重载」，为运算符提供了新的意义。

## 定义

要重载一个运算符也非常简单，将方法名改成你想要重载的运算符就行。下面的例子是一个 Vector3i 实现向量的部分运算。

```java
public class Vector3i {
    public val x as int;
    public val y as int;
    public val z as int;

    public this(x as int, y as int, z as int) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    public +(other as Vector3i) as Vector3i {
        return new Vector3i(this.x + other.x, this.y + other.y, this.z + other.z);
    }

    public -(other as Vector3i) as Vector3i {
        return new Vector3i(this.x - other.x, this.y - other.y, this.z - other.z);
    }

    public *(multiply as int) as Vector3i {
        return new Vector3i(this.x * multiply, this.y * multiply, this.z * multiply);
    }

    public ~(other as Vector3i) as int {
        return this.x * other.x + this.y * other.y + this.z * other.z;
    }

    public *(other as Vector3i) as Vector3i {
        return new Vector3i(this.y * other.z - this.z * other.y, this.z * other.x - this.x * other.z, this.x * other.y - this.y * other.x);
    }

    public implicit as string {
        return "Vector3i[" + this.x + ", " + this.y + ", " + this.z + "]";
    }
}
```

## 使用

```java
val va = new Vector3i(1, 2, 3);
val vb = new Vector3i(4, 5, 6);

println(va + vb); // 我们重载了加号运算符，现在可以用加号进行向量的加法运算了。
```

## 自动类型转换

在上面的例子出现了 `implicit`，这个关键字可以用来指定这个类能够自动转换成另一个类的对象。上面的例子我们添加了 Vector3i 向 string 的自动转换，这也意味着我们能够直接 print 这个对象。（先把对象转换为 string，再打印这个 string）

当然你也可以用 `as string` 来手动转型。

## IndexGet 与 IndexSet

IndexGet 和 IndexSet 运算符比较特殊，需采用 `[]` 和 `[]=`。以下是 stdlib 的 List 部分代码。

```java
// [Native] 表示调用内部 Java 方法，请忽略
[Native("getAtIndex")]
public [](index as usize) as T;

[Native("setAtIndex")]
public []=(index as usize, value as T) as T;
```

这样，对于一个 `List<T>` ，我们就可以用 `list[1]` `list[1] = bar` 的形式了。
