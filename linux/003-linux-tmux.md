# Tmux

## 1. Tmux 基本用法

### 1.1 启动与退出

```shell
$ tmux      # 启动

$ exit      # 退出
```

### 1.2 前缀键

默认前缀键 `Ctrl + b`， 帮助快捷键 `Ctrl + b  ?`, 按 ESC 或者 `q` 退出帮助。

## 2. 会话管理

```shell
# 1. 创建会话
$ tmux new -s <session-name>
# 2. 退出会话  Ctrl + b  d
$ tmux detach

# 3. 列出会话   Ctrl + b s
$ tmux ls

# 4. 接入会话
$ tmux attach -t 0
$ tmux attach -t <session-name>

# 5. 杀死会话
$ tmux kill-session -t 0
$ tmux kill-session -t <session-name>

# 6. 切换会话
$ tmux switch -t 0
$ tmux switch -t <session-name>

# 7. 重命名会话    Ctrl + b $
$ tmux rename-session -t 0 <new-name>
```

## 3. 窗格管理(Pane)

```
* Ctrl + b %                    左右分成两个 Pane
* Ctrl + b "                    上下分成两个 Pane
* Ctrl + b <arrow key>          按方向键切换 Pane
* Ctrl + b q                    显示 pane 编号
* Ctrl + b ;                    光标切换到上一个编号 pane
* Ctrl + b o                    光标切换到下一个编号 pane
* Ctrl + b x                    关闭当前 pane
* Ctrl + b z                    当前 pane 全屏显示，再次使用变回原来大小
* Ctrl + b !                    当前 pane 拆分为一个 独立 pane (常用)
* Ctrl + b Crtl+<arrow key>     按箭头方向调整窗格大小
* Ctrl + b {                    当前 pane 与上一个 pane 交换位置
* Ctrl + b }                    当前 pane 与下一个 pane 交换位置
* Ctrl + b Ctrl+o               所有 pane 向前移动一个位置，第一个变成最后一个
* Ctrl + b Alt+o                所有 pane 向后移动一个位置，最后一个变成第一个
```

## 4. 窗口管理 (window)

```
* Crtl + b  c                   新建新窗口，状态栏会显示多个窗口
* Ctrl + b  p                   切换到上一个窗口（按照状态栏显示的顺序）
* Crtl + b  n                   切换到下一个窗口
* Ctrl + b  <number>            切换到指定编号的窗口
* Ctrl + b  w                   从窗口列表中选择窗口
* Ctrl + b  ,                   重命名
```

## 5. 修改前缀键

```shell 
$ touch ~/.tmux.conf
```

添加如下内容:

```
set -g prefix C-a
unbind C-b
bind C-a send-prefix
```

使文件生效 

```shell
$ tmux start-server \; source ~/.tmux.conf
```
