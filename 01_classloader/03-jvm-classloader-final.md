# 类加载实例之常量

示例如下：

```java
import java.util.UUID;

public class MyClassloader{
    public static void main(String[] args){
        System.out.println(Parent.str);
    }
}

class Parent{
    // 注意一下两行分别执行
    public static final String str = "I'm Parent";
    // public static final String str = UUID.randomUUID().toString();
    static{
        System.out.println("Parent static block running");
    }
}
```

1. `Parent` 类的常量为编译器确定值时，即，`str = "I'm Parent"` 时，程序输出如下：

```java
I'm Parent
```

2. `Parent` 类的常量为运行时确定值时，即，`str = UUID.randomUUID().toString()` 时，程序输出如下：

```java
Parent static block running
I'm Parent
```

由[类的加载](./01-jvm-classloader.md) 可知，调用类的静态属性属于对类的主动使用，主动使用会导致类的初始化，但是当 `Parent` 类的字段值为编译器可确定的常量时，`Parent static block running` 并未输出，而当 `Parent` 类的字段值为运行时才能确定的值时，静态代码块得到执行。

常量在编译阶段会存入调用这个常量的方法所在类的常量池中，本质上，调用类并没有直接引用到常量所在的类，因此不会触发定义常量的类的初始化，此案例是指将 `Parent` 类的常量 `str="I'm Parent"` 存放到了 `MyClassLoader` 类的常量池中，之后，`Parent` 与 `MyClassLoader` 再无任何关系，甚至可以将 `Parent` 对应的 `class` 文件删除掉，对于程序的执行也没有任何问题。

当一个常量值并非在编译期确定时，那么其值就不会存入到调用类的常量池中，所以当程序调用时，就会导致主动使用这个常量所在的类，显然会导致这个类的初始化。


