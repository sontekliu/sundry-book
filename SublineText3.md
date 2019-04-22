# Subline Text 3  教程

### 1. There are no packages available for installation 错误

这个问题出现的原因很简单，就是获取 Sublime Text 3 的频道地址被网络了。我们可以在配置文件里找到这个地址。

打开 "Preferences -> Package Settings -> Package Control -> Settings Default" 配置文件，可以看到下面一段话

```js
    // A list of URLs that each contain a JSON file with a list of repositories.
    // The repositories from these channels are placed in order after the
    // repositories from the "repositories" setting
    "channels": [
        "https://packagecontrol.io/channel_v3.json"
    ],
```

那么要如何改呢？ 有两种方案

1. 把 "Preferences -> Package Settings -> Package Control -> Settings Default" 中的 "channels" 改成如下

```js
"channels":
    [
        "https://raw.githubusercontent.com/SuCicada/channel_v3.json/master/channel_v3.json"
    ]
```

当然了，这是不友好的，对吧。请看第二种

2. 在 "Preferences -> Package Settings -> Package Control -> Settings User" 中新添加一个项 "channels"

```js
"channels":
    [
        "https://raw.githubusercontent.com/SuCicada/channel_v3.json/master/channel_v3.json"
    ],
```
[参考地址](https://www.twle.cn/t/549)

### 2. 安装 markdown 插件

[参考连接](https://juejin.im/post/5c1609b2e51d45766d4e9bad)
[参考连接](https://blog.csdn.net/qazxswed807/article/details/51235792)


