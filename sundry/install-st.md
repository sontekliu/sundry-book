# install st

### 1. 下载源代码
```shell
$ cd ~/opt/gitspace
$ git clone https://git.suckless.org/st
$ cd st 
$ sudo make clean install 
```

### 2. 编辑字体和字号

```shell
static char *font = "Source Code Pro:pixelsize=18:antialias=true:autohint=true";
```

### 3. 打补丁

首先去[官网](https://suckless.org/st)下载对应的补丁到源码目录中，使用 patch 命令来进行打补丁。

```shell
$ wget http://st.suckless.org/patches/dracula/st-dracula-0.8.2.diff
$ patch < st-dracula-0.8.2.diff
$ sudo make clean install
```

