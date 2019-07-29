# Optional 类的使用

`Optional` 是 `JDK1.8` 之后引入的新类，主要解决 `Java` 中的空指针问题。

`Optional` 本质上是一个包含可选值的容器。

代码准备(getter setter 方法省略)：

```java
public class User {
    // 用户编号
    private String userNo;
    // 银行卡
    private BankCard bankCard;
}

public class BankCard {
    // 银行卡号
    private String cardNo;
    // 银行
    private Bank bank;
}
public class Bank {
    // 银行编码, 如 ICBC，CCB
    private String bankCode;
}
```

> 代码示例

```
// 在 JDK 1.8 之前，可能会有如下代码：

User user = ...;

if(null != user) {
    BankCard bankCard = user.getBankCard();
    if(null != bankCard) {
        Bank bank = bankCard.getBank();
        if (null != bank) {
            String bankCode = bank.getBankCode();
            if(null != bankCode) {
                System.out.println(bankCode.toUpperCase());
            }
        }
    }
}
```

---
### 1. 创建实例

`Optional` 构造方法是私有的，无法通过 `new` 关键字创建实例。但提供了三个工厂方法。

* 创建一个空对象(以 String 对象举例)

```
Optional&lt;String&gt; optional = Optional.empty();
```

* 当所容纳对象确定非空时(以 String 对象举例)

```
Optional&lt;String&gt; optional = Optional.of(str);
```

* 当所容纳对象不确定是否非空时(以 String 对象举例)

```
Optional&lt;String&gt; optional = Optional.ofNullable(str);
```


---
### 2. 正确使用

在说正确使用方式时，先说一下不规范的使用方式

```
// 不规范使用
User user = new User();
Optional&lt;User&gt; optional = Optional.of(user);
if(optional.isPresent()) {
    String userNo = optional.get().getUserNo();
    System.out.println(userNo);
}
```

正确的使用方式 :

```
// 正确使用
User user2 = new User();
Optional&lt;User&gt; optional2 = Optional.of(user2);
optional2.ifPresent(item -> System.out.println(item.getUserNo()));
```


---
### 3. 常用方法

* orElse() 返回对象值或者对象为空时，返回默认值

```java
    public void useOrElse() {
        User user = null;
        User user2 = new User("aaaaaa");
        User result = Optional.ofNullable(user).orElse(user2);
        System.out.println(result.getUserNo());
    }
```


* orElseGet() 返回对象值或者对象为空时，执行传入的 `Supplier` 函数式接口

```java
    public void useOrElseGet() {
        User user = null;
        User user2 = new User("aaaaaa");
        User result = Optional.ofNullable(user).orElseGet(() -> use2);
        System.out.println(result.getUserNo());
    }
```

当传入参数为 `NULL` 时，`orElse` 和 `orElseGet` 这两者无任何差别，但是当传入参数为非空时，二者还是有
差别的，代码如下：

```java
    public User createUser() {
        System.out.println("创建默认用户");
        return new User("default");
    }

    public void useOrElse() {
        User user = new User("S10001");
        User result = Optional.ofNullable(user).orElse(createUser());
        System.out.println(result.getUserNo());
    }

    public void useOrElseGet() {
        User user = new User("S20002");
        User result = Optional.ofNullable(user).orElseGet(() -> createUser());
        System.out.println(result.getUserNo());
    }
```

输出内容如下：

```
创建默认用户
S10001
S20002
```

当 `User` 对象均为非空时，`orElse` 和 `orElseGet` 无任何差别，均会调用 `createUser` 方法，而当 `User`
为非空对象时，`orElseGet` 方法不会执行 `createUser` 方法。

> 所以当执行密集型调用时，二者性能差异非常大。


* orElseThrow() 返回对象值或者对象为空时抛出异常

```java
    public void useOrElseThrow() {
        User user = null;
        Optional.ofNullable(user).orElseThrow(NullPointerException::new);
    }
```

* map 和 flatMap 转换值, 使用 Function 函数将对象进行转换
* filter  过滤选择需要的数据, 使用 Predicate 函数式接口进行过滤


---
### 4. 注意事项

`Optional` 一般不会作为类的属性使用，因为 `Optional` 没有实现 `Serializable` 接口，无法被序列化，再
就是一般也不会将其当做方法参数或者构造方法参数。

`Optional` 主要是用作返回类型，获取到对象的实例后，如果有值则取得这个值，否则进行一些替代行为。尤其
是它与 `Stream` 结合，可以构建流畅的 `API`。示例如下：

```
List&lt;User&gt; list = new ArrayList<>();
User user = list.stream().findFirst().orElse(new User());
```

---
### 5. 总结

* 三种 Optional 的实例化方式
* 正确使用 Optional
* 注意 orElse 和 orElseGet 的差异
* Optional 一般用于方法返回值


