# 方法引用

方法引用可以使程序更加简洁,可读性更高。

方法引用其实就是 `Lambda` 表达式的一个语法糖，可以把方法引用理解为方法的指针，方法引用指向一个已经实现的方法。

也就是说，如果一个已经存在的方法恰好时某个函数式接口抽象方法的实现, 那么就可以使用方法引用。

方法引用大体分为四种：

## 1. 类名::静态方法名

如果函数式接口的抽象方法的实现恰好可以通过调用一个静态方法来实现，那么就可以使用该类型的方法引用。如
: 如将列表中的所有字符串转换成整数

```java
List<String> list = Arrays.asList("1", "2", "3", "4", "5");
list.stream.map(Integer::parseInt)
           .forEach(item -> System.out.println(item));
```


---
## 2. 类名::实例方法名

如果函数式接口的抽象方法的实现恰好可以由这样一个实例方法的调用来实现：
抽象方法的第一个参数类型刚好是该实例的类型，抽象方法剩余的参数恰好可以当做实例方法的参数。
即调用的是`Lambda` 表达式第一个参数的实例方法，`Lambda` 表达式第一个参数以外的参数按顺序作为实例方法的参数传进去完成调用。

```java
List<String> list = Arrays.asList("hello", "abc", "jdk", "suixingpay", "user");
list.forEach(String::toUpperCase);
```


---
## 3. 对象名称::实例方法名

如果函数式接口的抽象方法的实现恰好可以通过调用一个实例的实例方法来实现，那么就可以使用该类型的方法引用。

```java

public class Product {
    private int price;
    // 品牌
    private String brand;

    public Product() {
    }

    public Product(int price, String brand) {
        this.price = price;
        this.brand = brand;
    }
    // getter setter
}

public class ProductCompare {

    public int compareByPrice(Product product1, Product product2) {
        return product1.getPrice() - product2.getPrice();
    }

    public int compareByBrand(Product product1, Product product2) {
        return product1.getBrand().compareToIgnoreCase(product2.getBrand());
    }
}

public class Test {
    public static void main(String[] args) {
        Product product = new Product(4000, "DELL");
        Product product2 = new Product(5002, "ASUS");
        Product product3 = new Product(6000, "THANKPAD");
        Product product4 = new Product(7000, "MAC");

        List<Product> list = Arrays.asList(product, product2, product3, product4);

        ProductCompare compare = new ProductCompare();

        list.sort(compare::compareByPrice);
        list.sort(compare::compareByBrand);
    }
}
```


---
## 4. 类名::new(构造方法引用)

示例 1

```java

public class Test {
    public static void main(String[] args) {
        // 函数式接口的实例可以使用 Lambda 表达式，方法引用和构造方法引用
        Supplier<String> supplier = String::new;
    }
}

```

示例 2

```java
public class Test {
    
    public static void main(String[] args) {
        Test test = new Test();
        test.getProduct(3000, "DELL", Product::new);
    }

    public Product getProduct(int price, String brand, BiFunction<Integer, String, Product> biFunction) {
        return biFunction.apply(price, brand);
    }
}
```



