---
title: "利用 Pqi 实现 Python 快速换源"
source: https://asurada.zone/post/change-python-pypisource/
date: 2020-06-06
updated: 2020-06-06
tags: [Python]
categories: [开发环境]
---

# 利用 Pqi 实现 Python 快速换源

pip 默认的软件源是国外的，下载起来速度很慢，我们可以将软件源换成国内的镜像源，大幅提高下载速度。本文介绍利用 `pqi` 这一小工具来实现简单快速地更换 pip 软件源。

`pqi` 是国内开发者 [yhf](https://github.com/yhangf) 开发的 [PyQuickInstall](https://github.com/yhangf/PyQuickInstall) 工具的简称，这款工具可以非常方便地帮助我们切换 pip 软件源，如果觉得好用的话，大家可以动动鼠标，在 Github 上面 Star 一下。

临时使用清华大学开源软件镜像源安装 pqi ：

```shell
pip install pqi -i https://pypi.tuna.tsinghua.edu.cn/simple
```

安装完成后，直接在命令行即可使用 pqi ：

```shell
//列举所有支持的PyPi源
pqi ls
//改变 PyPi源
pqi use <name>
//显示当前PyPi源
pqi show
//移除pip源
pqi remove <name>
//添加新的pip源
pqi add <name> <url>
//升级到最新版 pqi
pip install --upgrade pqi
```

如果决定使用上面提到过的清华大学镜像源，使用如下命令即可：

```shell
pqi use tuna
```

然后就可以享受飞一般的 pip下载速度了。
