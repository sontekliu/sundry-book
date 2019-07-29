# Lambda 表达式和函数式接口

## 1. Lambda 表达式

### 1.1 Lambda 示例代码

> 示例1

将列表中的数据输出到控制台:

```java
public class Test {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("张三", "李四", "王五", "赵六");
        // JDK1.5 之前
        for (int i=0; i< names.size(); i++) {
            System.out.println(names.get(i));
        }

        // JDK1.5 之后, 1.8 之前
        for (String item : names) {
            System.out.println(item);
        }

        // JDK1.8 之后 lambda 形式
        names.forEach((String item) -> {
            System.out.println(item);
        });
        // JDK8 支持类型推断，类型可省略；代码体只有一句，大括号可省略
        names.forEach((item) -> System.out.println(item));
        // 如果参数只有一个，括号也可以省略.
        names.forEach(item -> System.out.println(item));
        // JDK1.8之后，方法引用的形式
        names.forEach(System.out::println);
    }
}
```


---
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

---
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
### 1.2 什么是 Lambda 表达式

维基百科：
> Lambda: In programming languages such as Lisp, Python, Ruby, lambda is an operator 
> used to denote anonymous functions or closures, following the usage of lambda calculus


百度百科:
> Lambda 表达式是一个匿名函数，Lambda表达式基于数学中的λ演算得名，直接对应于其中的lambda抽象，
> 是一个匿名函数，即没有函数名的函数。Lambda表达式可以表示闭包。


`JDK1.8` 之前，`java` 是不支持函数式编程的，所谓函数式编程可以简单理解为将一个函数(或称为行为)作为参数进行传递。
在 `Java` 中我们通常使用的是面向对象编程，面向对象编程是对数据的抽象，而函数式编程是对行为的抽象。
`jdk1.8` 之后，我们不仅仅可以传递值，传递函数, 还可以返回一个函数。


---
### 1.3 Lambda 表达式语法

完整的结构

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


## 2. 函数式接口

### 2.1 函数式接口定义

> 有且仅有一个抽象方法的接口是函数式接口。一般使用 `@FunctionalInterface` 注解标明。

几点说明：

* 如果接口中包含 Object 类中的某个方法也是可以的。
* 也可以没有 @FunctionalInterface 注解声明

函数式接口的实现方式(见源码文档)：

* Lambda 表达式
* 方法引用
* 构造方法引用

---
### 2.2 JDK中常见的函数式接口

| 序号 | 函数名称  | 抽象接口          | 描述                        |
|------|-----------|-------------------|-----------------------------|
| 1    | Consumer  | void accept(T t)  | 接收一个参数，没有返回值    |
| 2    | Function  | R apply(T t)      | 接收一个参数，返回一个对象  |
| 3    | Predicate | boolean test(T t) | 接收一个参数，返回boolean值 |
| 4    | Supplier  | T get()           | 不接收参数，返回一个对象    |

* Predicate 

```java
public class PredicateTest {

    public static void main(String[] args) {
        // 整型数组
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        PredicateTest predicateTest = new PredicateTest();
        // 取出偶数并打印
        predicateTest.conditionFilter(list, item -> item % 2 == 0);
        System.out.println("------------");
        // 取出奇数并打印
        predicateTest.conditionFilter(list, item -> item % 2 != 0);
        System.out.println("------------");
        // 找出大于 5 的数字
        predicateTest.conditionFilter(list, item -> item > 5);
    }

    // 条件过滤, 但是具体什么条件，在使用时传入
    public void conditionFilter(List<Integer> list, Predicate<Integer> predicate) {
        list.forEach(item -> {
            if (predicate.test(item)) {
                System.out.println(item);
            }
        });
    }
}
```

从上面的代码可以看出，函数式编程提供了一种更高层次的抽象，之前写一个方法的时候，必须确定这个方法是
用来干什么的，有了 `Lambda` 之后，使得编写的方法更加抽象，更加通用。

可以在程序代码中编写一个通用的过滤方法，代码如下：

```
    public &lt;T&gt; List&lt;T&gt; filter(List&lt;T&gt; list, Predicate&lt;T&gt; predicate) {
        List&lt;T&gt; result = new ArrayList<>();
        for (T t: list) {
            if (predicate.test(t)) {
                result.add(t);
            }
        }
        return result;
    }
```

* Function
```
    public &lt;T, R&gt; List&lt;R&gt; map(List&lt;T&gt; list, Function&lt;T, R&gt; function) {
        List&lt;R&gt; result = new ArrayList<>();
        for (T t: list) {
            result.add(function.apply(t));
        }
        return result;
    }
```

### 2.3 Lambda 表达式的类型

在支持函数式编程的语言中，函数是被看做一等公民的，如：`javascript` 完全可以在一个 `js` 文件中写一
个函数，而在 `Java` 目前还不支持，函数必须依附于类，所以在 `Java` 中，函数式接口的实例依然是对象。

```java
public class ConsumerTest {
    public static void main(String[] args) {
        Consumer<String> consumer = item -> System.out.println(item);
        System.out.println(consumer.getClass().getName());
        System.out.println(consumer.getClass().getSuperclass());
        System.out.println(consumer.getClass().getInterfaces().length);
        System.out.println(consumer.getClass().getInterfaces()[0]);
    }
}

输出如下：
com.suixingpay.jdk8.functionalinterface.ConsumerTest$$Lambda$1/1452126962
class java.lang.Object
1
interface java.util.function.Consumer
```




