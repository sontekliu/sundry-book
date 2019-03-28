## Gitbook 教程

### 1. Gitbook简介
**GitBook** 是一个基于 `Node.js` 的命令行工具，可使用 `Git` 和 `Markdown` 来制作精美的电子书。**GitBook** 支持输出以下几种文档格式   

* 静态站点：GitBook默认输出该种格式
* PDF：需要安装gitbook-pdf依赖
* eBook：需要安装ebook-convert

### 2. Gitbook 安装

安装 **Gitbook** 之前需要先安装 `Node.js`。  
**Node.js 安装**

本教程主要倾向于在Linux平台下`Node.js`的安装，其他平台请另行百度或者Google。

* 首先[Node.js](http://nodejs.cn)官网下载对应平台的版本
* 解压 `tar -xvJf node-v6.10.3-linux-x64.tar.zx`
* 配置环境变量  
    - vim /etc/profile   
    - export NODE\_HOME=`*path*`/node-v6.10.3-linux-x64   
    - export PATH=$NODE\_HOME/bin:$PATH   
    - 使配置文件生效 source /etc/profile   
* 验证是否安装成功 `$ node -v`，若输出 `v6.10.3` ，则表示安装成功。

**Gitbook 安装**

安装完 `Node.js` 之后，默认安装了 `NPM`。下面我们使用 `NPM` 安装 **Gitbook**  

* npm install gitbook-cli -g
* gitbook -V 
    ```
    CLI version: 2.3.0
    GitBook version:3.2.2
    ```

### 3. Gitbook 目录折叠

如果使用 gitbook 编写大量的文章，目录不能折叠就显得杂乱无章，很难找到自己想要的章节，默认情况下 gitbook 的目录是展开的。
为此，可以使用 gitbook 插件，使其目录折叠。此小结主要介绍如何使 gitbook 目录折叠。

插件名称是：`toggle-chapters`。具体配置过程如下：  
首先在根目录下（即SUMMARY.md同级的目录），的配置文件 `book.json` 中添加插件配置，如下内容：
```json
{
    "plugins":["toggle-chapters"]
}
```

配置完成之后，需进行如下步骤：  
```shell
$ cd gitbook 目录
$ npm install gitbook-plugin-toggle-chapters  # 安装插件
$ gitbook build
$ gitbook serve
```

访问 `http://localhost:4000` 看插件是否生效。


	
#### Gitbook 参考资料

[Gitbook 入门教程](https://yuzeshan.gitbooks.io/gitbook-studying/content/index.html)   
[Gitbook 简明教程](http://www.chengweiyang.cn/gitbook/)  
[Gitbook 学习笔记](http://yangjh.oschina.io/gitbook/)  
[Gitbook 简单教程](http://gitbook.zhangjikai.com/)  


