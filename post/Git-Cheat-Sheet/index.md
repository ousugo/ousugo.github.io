---
title: "Git 常用命令备忘录"
source: https://asurada.zone/post/Git-Cheat-Sheet/
date: 2020-08-30
updated: 2020-08-30
tags: [Git]
categories: [开发环境]
---

# Git 常用命令备忘录

一些不太常用的 Git 命令往往隔一段时间不用就忘记了，特写此篇备忘。

## 配置 User 信息

### 添加 User 信息

```shell
git config [--global | --local] user.name 'your_name'
git config [--global | --local] user.email 'your_email'
```

-   默认为 local，所以只有需要设置全局时需要带 –global 参数，–local 参数可以直接省略。
-   单引号不是必须的，可以省略。一般只在输入的内容之中含有空格的时候需要使用单引号。

### 查看 User 信息

使用以下命令查看所有全局/局部配置信息：

```shell
git config --list [--local | --global | --system]
```

或者使用以下命令直接查看具体设置：

```shell
git config [--global | --local] user.name
git config [--global | --local] user.email
```

### global 与 local 优先级以及配置文件所在位置

local > global

local 配置位于当前仓库的 .git 文件夹下的 config 文件中。

global 配置位于当前用户 home 目录下的 .gitconfig 文件中。

## 独自使用 Git 常见使用场景

### git log 常用参数

-   git log –all 查看所有分支的历史
-   git log –graph 图形化查看 log 历史
-   git log –oneline 查看单行的简洁历史
-   git log -n 查看最近 n 条历史

### 删除不需要的分支

普通删除(如果当前分支没有被合并，不能直接删除)：

```shell
git branch -d 分支名
```

强制删除：

```shell
git branch -D 分支名
```

### 修改最近一次的 commit 内容

命令：

```shell
git commit --amend
```

### 修改最近一次 commit 的作者

```shell
git commit --amend --author="AuthorName <email@address.com>" --no-edit
```

其中 `no-edit` 的作用是表明此次修改仅修改作者，而不需要对 `commit message` 进行改动，所以不需要启动编辑器。

### 修改老旧 commit 的 message

1.  进入交互式变基策略修改：
    
    ```shell
    git rebase -i 想要修改的提交节点的父节点的 HASH 值
    ```
    
    如果想要修改第一次提交，也就是最早的那次提交，没有父节点的提交，使用命令：
    
    ```shell
    git rebase -i --root
    ```
    
2.  修改策略：
    
    在 vi 模式下将想要修改的 commit 前面的选项由 pick 变更为 r 或者 reword，这一选项的含义为此 commit 的内容不变，只修改其 commit message。
    
3.  修改 commit message
    

注意事项：某个 commit 修改后，它本身以及它后面所有 commit 的 hash 值都会发生变化，这是因为message 也是 commit 的属性，是确定 commit 的 hash 值的一个因子。其次，second\_commit 有个 parent 的属性，parent 的内容发生了变化，因此，second\_commit 的 hash 也变了。

如果最早的提交也即 root commit 发生了变化，那么会产生一个新的 root commit，即出现了两个祖宗 commit。

### 把连续的多个 commit 整理成一个

1.  进入交互式变基策略修改：
    
    ```shell
    git rebase -i 想要修改的提交节点的父节点的 HASH 值
    ```
    
    如果想要修改第一次提交，也就是最早的那次提交，没有父节点的提交，使用命令：
    
    ```shell
    git rebase -i --root
    ```
    
2.  修改策略：
    
    将准备被合并的 commit 前面的选项由 pick 改为 s 或者 squash，这一选项的含义为继续使用这一 commit，但是将其合并到前面(比它老)的那一个 commit 里面去，想要合并哪几个就将这几个前面的选项都改成 s。
    
3.  修改 commit message
    

### 把间隔的多个 commit 整理成一个

原理和方法同“把连续的多个 commit 整理成一个”是一样的，只是 rebase 策略的不一样：

-   将想要整理的 commit 通过剪切，按照自己想要的顺序移动到一起，排在上面的是越老的
-   将想要整理的 commit 前面的选项变更为 s
-   最后这些 commit 会同上方最近的一个选项为 pick 的 commit 合并

会用到的 vim 快捷键：

```less
dd	//剪切一行，也可以用来删除一行
dw	//删除一个单词
u	//撤销
p	//粘贴
```

### 比较暂存区和 HEAD 所含文件的差异

HEAD 所含文件就是最新一个 commit 内的文件内容

```ada
git diff --cached
```

### 比较工作区和暂存区所含文件的差异

```ebnf
git diff
```

后面可以接“– 单个或多个文件名”来查看特定文件的差异

### 比较工作区和 HEAD 所含文件的差异

```ebnf
git diff HEAD
```

### 让暂存区恢复成和 HEAD 一样

```ebnf
git reset HEAD
```

git reset 默认的参数为 –mixed

### 让工作区的文件恢复为和暂存区一样

```ada
git checkout -- 文件名
```

Git 2.23 版本以后使用 `git restore`命令来代替`git checkout`功能

### 取消暂存区部分文件的更改

将暂存区部分文件取消暂存的意思是所做的改动继续保留在工作目录，暂存区部分的内容同 HEAD 保持一致。

```pgsql
git reset HEAD --文件名
```

Git 新版本使用：

```stata
git restore --staged <file>
```

两条命令作用是一样的。

### 消除最近的几次提交

```pgsql
git reset --hard <hash>
```

将工作区和暂存区的内容都恢复到 hash 值所对应的那个 commit 的状态。

### 看不同提交的指定文件的差异

```css
git diff [from_commit] [to_commit]
```

### 正确删除文件的方法

```xml
git rm <filename>
```

## 本地与远程仓库的交互

### 让 Fork 的仓库同源仓库保持更新

#### 添加源仓库

```routeros
git remote add upstream 源仓库链接
```

#### 同步更新

1.  如果在另一个分支上，就切换到 master 分支

```crmsh
git checkout master
```

2.  从源仓库抓取最新更改后，合并到本地 master 分支

```ebnf
git pull upstream
```

3.  将本地 master 分支推送到自己 Fork 的远程仓库 origin

```maxima
git push origin
```

#### 进阶版

1.  将 master 分支的上游仓库设置为源仓库

```crmsh
git branch --set-upstream-to=upstream/master master
```

2.  将 push 命令推送的默认仓库设置为自己 Forked 的仓库 origin

```lua
git config --local remote.pushDefault origin
```

3.  搞定之后，工作流程为更加简单：

```shell
git checkout master
git pull	//默认从源仓库拉取最新的更新
git push	//将拉取的更新推送到 Forked 的仓库
```

### 推送一个新的分支到远程仓库

\-u 是 –set-upstream 的简写，该标记会为之后轻松地推送与拉取配置分支

```shell
git push -u remote_name new_branch_name
```

### 清理本地无效的远程追踪分支

利用选项 –prune

```css
git remote update -p
```

### 远程删除了分支本地也想删除

```ebnf
git remote prune origin
```

### 删除远程分支

```maxima
git push origin --delete <remote-branchname>
```

或者

```elixir
git push origin :<remote-branchname>
```

### 从远程分支中创建并切换到本地分支

```xml
git checkout -b <branch-name> <remote-name>/<branch-name>
```

## 参考链接

[Git的奇技淫巧](https://github.com/521xueweihan/git-tips)
