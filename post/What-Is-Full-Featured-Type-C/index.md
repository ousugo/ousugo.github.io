---
title: "什么是全功能 Type-C 及显示器所需带宽如何计算"
source: https://asurada.zone/post/What-Is-Full-Featured-Type-C/
date: 2021-01-24
updated: 2021-01-24
tags: [硬件, 显示器, USB, Type-C]
categories: [硬件]
---

# 什么是全功能 Type-C 及显示器所需带宽如何计算

本文主要介绍了如何计算特定分辨率和刷新率显示器所需带宽以及对全功能 Type-C 的一些简单科普。

## 背景

最近动了换新笔记本和显示器的念头，自己想换一台 4K 显示器，在了解相关知识的时候，经常看到要注意视频输出接口带宽的提醒。其中各种概念多且杂，初接触很容易懵圈，所以我整理汇总了一些网上的资料，以作此文。

文中如有不对或不妥的地方，欢迎评论留言指出错误，不甚感激。

## 4K@60Hz 显示需要多少带宽

我们这里以一块 4K、8 bit、60 Hz 的屏幕举例，这块显示屏有 3840 \* 2160 个像素点，每个像素点又由红绿蓝三个子像素构成 ，每个子像素需要 8 bit 的数据，刷新率为 60 Hz，也就是每秒需要刷新 60 次，那么每秒需要传输的数据量就是 3840 \* 2160 \* 3 \* 8 \* 60 = 11.94 Gbit，需要的带宽就是 11.94 Gbps。

上述这些数据只是我们能够在显示屏上看到的内容，实际上在传输相关画面数据的时候，还需要考虑到防止画面撕裂，以及画面对齐等等需求，需要传输比实际显示画面更多的数据，所以实际传输的数据会比上面计算的更多。

![显示器实际传输数据](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/7-%E6%98%BE%E7%A4%BA%E5%99%A8%E5%AE%9E%E9%99%85%E4%BC%A0%E8%BE%93%E6%95%B0%E6%8D%AE.png)

下面这张来自 B 站 Up 主[硬件茶谈](https://space.bilibili.com/14871346#/)的表格详细列出了市面上常见分辨率及刷新率的组合分别所需要的带宽，其中还有各种显示器接口的相容性：

![显示接口相容表](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/8-%E6%98%BE%E7%A4%BA%E6%8E%A5%E5%8F%A3%E7%9B%B8%E5%AE%B9%E8%A1%A8.png)

显示器带宽具体的计算原理可以查看 Up 主的[视频-【硬件科普】4K144Hz需要传输多少数据，教你如何正确的计算显示器的带宽需求？](https://b23.tv/rbYOKW)，原理及计算方法都讲得非常清楚，动画特效也是一流。

在此也强烈安利下 Up 主，最早是在知乎关注他的，在知乎笔记本话题下相当活跃，后期转型成 B 站 Up 主，视频质量极高，讲解概念深入浅出，动画精致流畅，看他的视频可谓一种享受。我从 Up 主的视频里学到了很多硬件相关基础知识。而且 Up 主现在还只是一名在校大学生，让人不服不行，在此就祝愿 Up 主早日发大财吧。

## 什么是全功能 Type-C

同时具有**数据传输、充电、显示**三项功能的 Type-C 就称之为全功能 Type-C。而之所有会有「全功能」和「非全功能」的区分，是因为 Type-C 只是个**接口形状**。Type-C 接口是能够支持 100W PD 供电、DP 显示输出、USB 数据传输等功能没错，但 **Type-C 接口的功能都是可以被增减的**，所以并不是所有 Type-C 接口都同时具备数据传输、充电、显示三项功能。下图引用自[【从入门到放弃】关于Type-C/雷电3接口的两万字科普（上篇） - 知乎](https://zhuanlan.zhihu.com/p/101434503) ：

![Type-C 不同功能说明](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/2-Type-C%E4%B8%8D%E5%90%8C%E5%8A%9F%E8%83%BD.png)

每台电脑上面所配备的 Type-C 接口所具备的功能是不尽相同的，厂商基于成本或是其他方面的考虑，配备的 Type-C 可能只支持 USB 数据传输功能 ，不能充电、显示，也可能配备只支持 DP 显示和数据传输的 Type-C 口，不能充电。下图引用自[【从入门到放弃】关于Type-C/雷电3接口的两万字科普（上篇） - 知乎](https://zhuanlan.zhihu.com/p/101434503) ：

![Type-C 现状](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/1-TypeC%E5%8A%9F%E8%83%BD%E4%B8%8D%E9%BD%90.png)

## Type-C 的「数据传输」带宽

Type-C 接口还有一个重要的参数就是 USB 版本，USB 版本越高，带宽也就越高，理论上带宽越高，传输速度越快。不过 USB 版本的命名经过了几次大的修改，命名规范变来变去，对普通消费者识别起来极不友好。下面两张图分别引用自[USB type C at a glance: What the port can do, what it can’t do and why it has nothing to do with USB 3.0](https://basic-tutorials.com/usb-type-c-at-a-glance-what-the-port-can-do-what-it-cant-do-and-why-it-has-nothing-to-do-with-usb-3-0/)及[维基百科-USB](https://zh.wikipedia.org/wiki/USB)。其中 USB3.2 Gen 2x2 的 Logo 以维基百科的为准。

![USB 版本说明](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/3-USB%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E.png)

![USB 版本及速率(维基百科)](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/4-USB%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91.png)

通过这两张图，我们能够比较清楚地知道各个 USB 版本对应的传输速率及兼容接口。目前笔记本上 Type-C 接口的主流速率还是 USB3.2 Gen 1 也就是 5Gbps，理论速度约合 500 MB/s。

**但需要注意的是，多数情况下，「USB 几点几」这样的称呼，仅仅只是用于描述接口的「数据传输」速度，不代表这个接口其他任何能力，如能否PD充电、能否显示。**

## 5Gbps 的 Type-C 无法满足 4K@60Hz 输出？

这个问题也是我在没了解相关知识之前相当困惑的一个地方——当我看到心仪的笔记本只有 USB 3.2 Gen1 接口和 HDMI1.4b 的接口，而两者的带宽都远小于 4K@60Hz 所需的 12.54 Gbps 时，我当时同样以为外接 4K@60Hz 显示器是不可能实现的了。

这里面的问题就在于我们**混淆了 Type-C 接口的「数据带宽」和「显示带宽」**，我们日常习惯说一个 Type-C 的带宽是 3.1 Gen1 或者 Gen2、5Gbps 或者10Gbps，这时候说的其实都是这个接口的 USB 数据带宽，只能代表这个接口接 U盘/移动硬盘之类的存储进行数据传输时所能达到的最大速度。

Type-C能够整合显示输出功能，主要是借助了DP Alt Mode（替代模式）功能，借助Type-C正反面诸多针脚中的几个定义为显示接口。通俗点说可以理解为单独做了个 DP 显示总线放在 Type-C 口上，所以数据带宽是 5Gbps 还是10Gbps 并**不代表显示带宽也只有这么多**。以目前 Type-C 的规范来看，当我们说一个 Type-C 支持显示输出时，那么这个接口**至少可以支持 DP1.2 标准**，也就是说拥有独立的约 21Gbps 的显示带宽，因为 DP1.2 中有 20% 的编码是用于校验的，所以真正能用来传输视频信号的带宽只有 21.60 \* 0.8 = 17.28 Gbps，但这也已经**完全可以满足 4K@60Hz 的传输要求了**。下图引用自[【拯救者科普】笔记本外接显示器需要注意什么？ - 知乎](https://zhuanlan.zhihu.com/p/98911511)：

![Type-C 显示拥有独立的显示带宽](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/5-USBC%E7%8B%AC%E7%AB%8B%E6%98%BE%E7%A4%BA%E5%B8%A6%E5%AE%BD.png)

我们现在关于 5Gbps 的 Type-C 能否满足 4K@60Hz 画面传输的问题有了明确的答案，只要是**支持显示输出的 Type-C 口，就可以支持输出 4K@60Hz**。

## 5Gbps 的 Type-C 的不足之处

Type-C 显示输出虽然是 DP/HDMI 显示，但实际上还是借用了数据传输的一部分引脚。当一个支持 DP 显示输出的 Type-C ，而它的数据带宽是 USB3.1 Gen1 5Gbps 时，那么此时是无法**通过一根线**同时实现理应互不干扰的 4K@60Hz 和 USB3.0 传输的，下图引用自[【从入门到放弃】关于Type-C/雷电3接口的两万字科普（上篇） - 知乎](https://zhuanlan.zhihu.com/p/101434503) ：

![4K 显示输出的几种场景](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/6-4K%E6%98%BE%E7%A4%BA%E8%BE%93%E5%87%BA%E7%9A%84%E5%87%A0%E7%A7%8D%E5%9C%BA%E6%99%AF.png)

因此在一些显示器的详情界面会注明——当显示器上的 USB 设置为 3.2Gen1 时，只能输出 4K@30Hz 的画面，如果想要输出 4K@60Hz 需要将 USB 设置为 2.0 或关闭。所以如果我们使用的显示器有拓展 USB 口，而我们是使用 Type-C 直连显示器，为了保证使用体验，我们需要手动将显示器上的 USB 设置为 USB2.0，此时显示器上的 USB 更适合用来接键鼠，文件传输之类的还是直连笔记本会比较快。

![显示器详情界面的提示](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/9-%E6%98%BE%E7%A4%BA%E5%99%A8%E8%AF%A6%E6%83%85%E7%95%8C%E9%9D%A2%E7%9A%84%E6%8F%90%E7%A4%BA.png)

## 参考链接

[【从入门到放弃】关于Type-C/雷电3接口的两万字科普（上篇） - 知乎](https://zhuanlan.zhihu.com/p/101434503)

[【从入门到放弃】关于Type-C/雷电3接口的两万字科普（下篇） - 知乎](https://zhuanlan.zhihu.com/p/101486153)

[【拯救者科普】笔记本外接显示器需要注意什么？ - 知乎](https://zhuanlan.zhihu.com/p/98911511)

[USB type C at a glance: What the port can do, what it can’t do and why it has nothing to do with USB 3.0](https://basic-tutorials.com/usb-type-c-at-a-glance-what-the-port-can-do-what-it-cant-do-and-why-it-has-nothing-to-do-with-usb-3-0/)

[维基百科-USB](https://zh.wikipedia.org/wiki/USB)

[视频-【硬件科普】4K144Hz需要传输多少数据，教你如何正确的计算显示器的带宽需求？](https://b23.tv/rbYOKW)

[DP vs HDMI 谁才是游戏玩家最佳选择](https://diy.pconline.com.cn/1374/13748807.html)
