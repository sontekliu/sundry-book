# Stream

流由 3 部分构成：

* 源
* 零个或多个中间操作
* 终止操作

流操作的分类：

* 惰性求值, 中间操作是惰性求值, 如: filter
* 及早求值, 最后的操作是及早求值, 如：count

流的创建方式：

* 通过静态方法，Stream.of();
* 通过数组来创建 Arrays.stream(array[])
* 使用集合创建 list.stream
