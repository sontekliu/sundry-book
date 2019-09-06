# Git 整合

先上图：
![Git Environment](./images/git_environment.png)

前期准备工作:
    1. 阿里云安装 gitbook
    2. 阿里云配置 git

1. 首先在阿里云和 Linode 对应目录下创建对应的裸库

* 代码库  `/home/git/gitRepo/code`
* Gitbook库 `/home/git/gitRepo/gitbook`
* 其他    `/home/git/gitRepo/sundry`

```
cd /home/git/gitRepo/gitbook/
git init --bare git-book.git
```

2. 创建新的项目或者从 Github 上 clone 已存在的项目

```
已存在项目
git clone git@github.com:sontekliu/git-book.git

创建新项目
git init go-book
```

3. 将项目提交到阿里云

* 已存在项目

```
git remote add aliyun git@39.105.28.88:/home/git/gitRepo/gitbook/git-book.git
git push aliyun master:master
```

* 新项目
```
git clone git@39.105.28.88:/home/git/gitRepo/gitbook/go-book.git
```

4. 将 origin 的地址改成阿里云地址, 即本地只提交到阿里云，提高速度

新项目是直接从阿里云clone下来的，origin 地址默认就是阿里云地址，无需修改。

针对已存在的项目，编辑本项目下的 `.git/config` 文件，修改如下：

愿内容（注意已做删减）：
```
[remote "origin"]
	url = git@github.com:sontekliu/git-book.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[remote "aliyun"]
	url = git@39.105.28.88:/home/git/gitRepo/gitbook/git-book.git
	fetch = +refs/heads/*:refs/remotes/aliyun/*
```
修改成如下内容：

```
[remote "origin"]
	url = git@39.105.28.88:/home/git/gitRepo/gitbook/git-book.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```
去掉了刚刚添加的 `aliyun` ，并将原来 `Github` 的地址更换成了阿里云地址

5. 将项目备份多份

大体实现思路就是，当提交代码到阿里云时，利用 git 的勾子，将代码再次提交到 Github、码云、Coding 等

5.1 首先在对应平台创建相应的项目
    到相应平台创建相应的项目即可
5.2 将提交到阿里云裸库的项目 clone 到阿里云本地
    * 代码库  `/home/git/gitspace/code`
    * Gitbook库 `/home/git/gitspace/gitbook`
    * 其他    `/home/git/gitspace/sundry`

    ```
    $ cd /home/git/gitspace/gitbook
    $ git clone /home/git/gitRepo/gitbook/git-book.git
    ```
5.3 配置 git 
```
git config --global user.name  sontek
git config --global user.email sontek@yeah.net
```

5.4 修改项目地址，使其指向多个地址

修改 `clone` 到阿里云本地仓库的文件，`/home/git/gitspace/gitbook/git-book/.git/config`

愿内容为:

```
[remote "origin"]
        url = /home/git/gitRepo/gitbook/git-book.git
        fetch = +refs/heads/*:refs/remotes/origin/*
```
修改为:

```
[remote "origin"]
        url = /home/git/gitRepo/gitbook/git-book.git
        url = git@github.com:sontekliu/git-book.git 
        url = git@gitee.com:sontekliu/git-book.git
        url = git@173.230.144.249:/home/git/gitRepo/gitbook/git-book.git
        fetch = +refs/heads/*:refs/remotes/origin/*
```
此处添加了三个地址，分别是 `Github`，`码云`，个人VPS, 此处注意一定要将阿里云 `git` 用户的密钥都添加
到对应服务器，以便在代码提交的时候能够免密

5.5 编写 Git 勾子脚本使其自动化

在阿里云的本地仓库中编写脚本，使其自动化提交。

```
$ cd /home/git/gitRepo/gitbook/git-book.git/hooks/
$ cp post-update.sample post-update
$ vim post-update

#!/bin/sh
#
# An example hook script to prepare a packed repository for use over
# dumb transports.
#
# To enable this hook, rename this file to "post-update".

# exec git update-server-info
unset GIT_DIR
GIT_DIR=/home/git/gitspace/gitbook/git-book
cd $GIT_DIR
echo `pwd`
# 拉取代码
git pull origin master 
# 部署
sh $GIT_DIR/deploy.sh >/dev/null 2>&1 &
# 提交到远程
git push origin >&- 2>&- &
# git push origin >/dev/null 2>&1 &
```
