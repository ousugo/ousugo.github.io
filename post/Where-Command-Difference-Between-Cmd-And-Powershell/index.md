---
title: "CMD 和 PowerShell 中 where 命令的区别"
source: https://asurada.zone/post/Where-Command-Difference-Between-Cmd-And-Powershell/
date: 2025-01-19
updated: 2025-01-19
tags: [Windows, PowerShell, CMD]
---

# CMD 和 PowerShell 中 where 命令的区别

事情的起因是我想解决在 Windows Terminal 中使用 `code .` 命令打开的不是 VSCode 而是 Cursor 的问题。

## where 命令的介绍

在 Windows 命令提示符（CMD）中，`where` 命令用于查找与指定名称匹配的可执行文件的位置，它可以帮助你确定某个程序或文件在系统中的具体路径。我的使用场景是用它来查找 `code` 命令对应的可执行文件的路径。

## 现象

在 CMD 中执行 `where code` 命令，结果符合预期，结果如下：

![CMD 中执行结果](https://pub-40b650f1eea44087adc96bf9abf9c38d.r2.dev/1-CMD%E4%B8%AD%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9E%9C.png)

但在我常用的 PowerShell 中执行的结果则是没有任何输出：

![PowerShell 中执行结果](https://pub-40b650f1eea44087adc96bf9abf9c38d.r2.dev/2-PS%E4%B8%AD%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9E%9C.png)

## 原因

这是因为在 PowerShell 中 `where` 表示的不再是 `where.exe`，而是 `Where-Object` cmdlet 的别名。而 `Where-Object` 的作用是根据属性值从集合中选择对象，和 Linux 中的 `grep` 命令作用类似，通常是结合管道一起使用的。现在直接执行 `where code` 命令，因为管道的上游没有任何输入，自然也不会输出任何内容。

![where 命令在 PowerShell 中的结果](https://pub-40b650f1eea44087adc96bf9abf9c38d.r2.dev/4-where%E5%91%BD%E4%BB%A4%E5%9C%A8PS%E4%B8%AD%E7%9A%84%E7%BB%93%E6%9E%9C.png)

在 PowerShell 中使用 `where` 命令的正确姿势应该是使用 `where.exe` ：

![在 PowerShell 中使用 where.exe](https://pub-40b650f1eea44087adc96bf9abf9c38d.r2.dev/3-%E5%9C%A8PS%E4%B8%AD%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8whereexe.png)

或者直接在 PowerShell 中使用 `Get-Command` 命令：

![Get-Command 命令效果](https://pub-40b650f1eea44087adc96bf9abf9c38d.r2.dev/6-Get-Command%E5%9C%A8PS%E4%B8%AD.png)

但显然 `Get-Command` 命令的输出显示方式远不如 `where` 命令直观。

## 参考链接

[where](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/where)

[Where-Object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.4)

[Get-Command](https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/get-command?view=powershell-7.4)

[Why doesn’t the ‘where’ command display any output when running in PowerShell?](https://stackoverflow.com/questions/16775686/why-doesnt-the-where-command-display-any-output-when-running-in-powershell)
