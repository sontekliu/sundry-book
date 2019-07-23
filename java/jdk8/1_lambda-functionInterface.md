# Lambda 表达式和函数式接口

### 1. Lambda 表达式

##### 1. Lambda 示例代码

> 示例1

将列表中的数据输出到控制台:

```java
public class Test {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("张三", "李四", "王五", "赵六");
        // JDK1.5 之前
        for (int i=0; i< names.size(); i++) {
            System.out.println(names[i]);
        }

        // JDK1.5 之后, 1.8 之前
        for (String item : names) {
            System.out.println(item);
        }

        // JDK1.8 之后 lambda 形式
        names.forEach(item -> System.out.println(item));
        // JDK1.8之后，方法引用的形式
        names.forEach(System.out::println);
    }
}
```

> 示例2

遍历当前目录下面的隐藏文件：

```java
public class Test {
    public static void main(String[] args) {
        File file = new File(".");
        // JDK 1.8 之前
        file.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                return pathname.isHidden();
            }
        });

        // JDK 1.8 之后
        file.listFiles(File::isHidden);
    }
}
```

> 示例3

启动一个线程

```java
public class Test {
    public static void main(String[] args) {
        // 传统启动线程方式
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("thread execute...");
            }
        }).start();

        // JDK 1.8 之后
        new Thread(() -> System.out.println("thread execute...")).start();

    }
}
```

---
##### 2. 什么是 Lambda 表达式

维基百科：
> Lambda: In programming languages such as Lisp, Python, Ruby, lambda is an operator 
> used to denote anonymous functions or closures, following the usage of lambda calculus


百度百科:
> Lambda 表达式是一个匿名函数，Lambda表达式基于数学中的λ演算得名，直接对应于其中的lambda抽象，
> 是一个匿名函数，即没有函数名的函数。Lambda表达式可以表示闭包。


在 `Java` 中，我们无法将函数作为参数传递给一个方法，也无法声明返回一个函数的方法。而在 `Javascript` 中，函数参数是一个函数，
返回值是另一个函数的情况是非常常见的，`javascript` 是一门非常典型的函数式语言。

`Java8` 之后，我们不仅可以传递值，还可以传递行为(函数式接口)。

---
##### 3. Lambda 表达式语法

```
(Type1 arg1, Type2 arg2, ...) -> {
    ...
}
```
一般情况下，`Java` 默认是支持类型推断的，所以括号的类型可以省略。如下：

```
(arg1, arg2, ...) -> {
    ...
}
```
如果参数只有一个，括号也可以省略，如果参数为空，括号不能省略:

```
# 一个参数
arg -> {}

# 无参数
() -> {}
```

### 2. 函数式接口

##### 1. 函数式接口定义

一个函数式接口必须满足如下条件(见源码):

* 必须是一个接口，并且该接口只有一个抽象方法。
* 可以包含 `Object` 类里的某个抽象方法
* 可以不需要 `@FunctionInterface` 声明

##### 2. JDK中常见的函数式接口























