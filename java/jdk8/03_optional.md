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
Optional<String> optional = Optional.empty();
```

* 当所容纳对象确定非空时(以 String 对象举例)

```
Optional<String> optional = Optional.of(str);
```

* 当所容纳对象不确定是否非空时(以 String 对象举例)

```
Optional<String> optional = Optional.ofNullable(str);
```


---
### 2. 正确使用

在说正确使用方式时，先说一下不规范的使用方式

```
// 不规范使用
User user = new User();
Optional<String> optional = Optional.of(user);
if(optional.isPresent()) {
    String userNo = optional.get().getUserNo();
    System.out.println(userNo);
}
```

正确的使用方式 :

```
// 正确使用
User user2 = new User();
Optional<User> optional2 = Optional.of(user2);
optional2.ifPresent(item -> System.out.println(item.getUserNo()));
```


---
### 3. 常用方法

* orElse() 返回对象值或者对象为空时，返回默认值
* orElseGet() 返回对象值或者对象为空时，执行传入的 `Supplier` 函数式接口
* orElseThrow() 返回对象值或者对象为空时抛出异常


---
### 4. 注意事项

`Optional` 一般不会作为类的属性使用，因为 `Optional` 没有实现 `Serializable` 接口，无法被序列化，再
就是一般也不会将其当做方法参数或者构造方法参数。

`Optional` 主要是用作返回类型，获取到对象的实例后，如果有值则取得这个值，否则进行一些替代行为。尤其
是它与 `Stream` 结合，可以构建流畅的 `API`。示例如下：

```
List<User> list = new ArrayList<>();
User user = list.stream().findFirst().orElse(new User());
```

---
### 5. 总结

* Optional 的实例化
* Optional 的正确使用
* Optional 的常用方法
* Optional 的注意事项


https://www.cnblogs.com/zhangboyu/p/7580262.html
