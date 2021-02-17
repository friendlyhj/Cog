# 自定义类

> 对象：对象是类的一个实例（对象不是找个女朋友），有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
>
> 类：类是一个模板，它描述一类对象的行为和状态。

## 定义一个类

```java
public class Animal {
    public var age as int; // 类的实例字段，若为 val，则这个字段是 final 的，不得再次修改

    public this(age as int) { // 方法名若为 this，即为这个类的构造方法
        this.age = age;
    }

    public this() { // 重载构造方法
        this(10);
    }
    
    public grow() as void { // 方法，定义与函数差不多，不过你可以在里面使用 this，代表当前对象
        this.age += 1;
    }

    public get bigAge as int { // 其实也是方法，不过 getter 的形式存在
        return this.age * 2;
    }

    public set bigAge as int { // setter
        this.age = $ / 2; // 参数 $ 为要 set 的参数
    }

    public showAge() as void { 
        println("The animal's age is " + age);
    }
}
```

## 使用这个类

无需任何导入。`public` 代表所有脚本均可以使用这个类、字段、方法。如果把前面的 `public` 改为 `private`，则只有该脚本内能使用。

```zenscript
val animalOne = new Animal(8); // 使用 new 关键词实例化这个类，新建新的这个类的一个对象
val animalTwo = new Animal();

animalOne.showAge(); // 使用方法
animalTwo.showAge();

animalOne.grow();
animalTwo.grow();

println(animalTwo.bigAge); // 使用这个类的 getter
```
