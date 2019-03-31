# 类加载实例之常量

示例如下：

```java
public class MyClassloader{
    public static void main(String[] args){
        // 如下两行语句分别执行，当执行第一句时，请注释掉第二句，当执行第二句时，注释掉第一句
        System.out.println(Child.str);
        System.out.println(Child.str2);
    }
}

class Parent{
    public static String str = "I'm Parent";
    static{
        System.out.println("Parent static block running");
    }
}

class Child extends Parent{

    public static String str2 = "I'm Child";
    static{
        System.out.println("Child static block running");
    }
}
```

1. 当执行 main 方法中的第一句(`System.out.println(Child.str)`)时，程序输出如下：

```java
Parent static block running
I'm Parent
```

由[类的加载](./01-jvm-classloader.md) 可知，调用类的静态属性属于对类的主动使用，主动使用会导致类的初始化，但是程序中并没有输出 `Child static block running` 内容。由此可知，对于静态字段来说，只有直接定义了该字段的类才会被初始化。

2. 当执行 main 方法的第二句(`System.out.println(Child.str2)`)时，程序输出如下：

```java
Parent static block running
Child static block running
I'm Child
```

由[类的加载](./01-jvm-classloader.md) 可知，调用类的静态属性属于对类的主动使用，主动使用会导致类的初始化，但是程序中首先输出 `Child static block running` 内容。由此可知，当一个类在进行初始化时，要求其父类必须完全初始化才行。
