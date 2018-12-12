# Gitbook 集成到 github pages

本文采用一个项目实例讲解，项目地址：https://github.com/sontekliu/book_git  
此项目之前已创建，此项目为最基本的 Gitbook 项目，只不过是存放在 github 上而已。

### 1. 首先创建 gh-pages 分支
执行如下命令创建分支，并删除一些不必要的文件。注意分支名称只能是 `gh-pages`
```
$ git checkout -b gh-pages
$ git rm --cached -r .
$ git clean -df
```
此时，此项目下面只剩下 `_book` 目录，注意之前已经将 `_book` 目录添加到 `.gitignore` 文件中。

### 2. 将 _book 下的内容添加 gh-pages 中
首先，设置一些忽略，然后将 `_book` 下面的内容添加到分支中。
```
$ echo "*~" > .gitignore
$ echo "_book" >> .gitignore
$ git add .gitignore
$ git commit -m "ignore some files"
```
将 `_book` 下的内容添加到分支中：
```
$ cp -r _book/* .
$ git add .
$ git commit -m "publish book"
$ git push -u origin gh-pages
```







