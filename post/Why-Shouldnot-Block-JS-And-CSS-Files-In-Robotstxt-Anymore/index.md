---
title: "为什么 robots.txt 不应该再屏蔽 JS 和 CSS 了"
source: https://asurada.zone/post/Why-Shouldnot-Block-JS-And-CSS-Files-In-Robotstxt-Anymore/
date: 2020-08-31
updated: 2020-08-31
tags: [SEO]
categories: [个人博客]
---

# 为什么 robots.txt 不应该再屏蔽 JS 和 CSS 了

根据谷歌官方的建议，robots.txt 已经不需要再屏蔽 JavaScript、CSS 文件了。

## 屏蔽是过时的做法

在前期查找相关资料的时候，我注意到不少博客都建议在 `robots.txt` 中禁止搜索引擎抓取网站的 JavaScript、CSS 文件，但其中大多数文章并没有给出为什么要这样做的具体理由。

但我觉得现在已经有理由相信这种做法是**过时且不必要** 的。谷歌在其官方的[搜索引擎优化 (SEO) 新手指南](https://support.google.com/webmasters/answer/7451184)中的「让 Google 以用户视角查看网页」章节中明确提到：

> 当 Googlebot 抓取某个网页时，它应[以普通用户查看网页的方式来查看它](https://webmasters.googleblog.com/2014/05/understanding-web-pages-better.html)。为了实现最佳的呈现和索引编制效果，请始终允许 Googlebot 访问您网站所用的 JavaScript、CSS 和图片文件。如果您网站的 robots.txt 文件禁止抓取这些资源，则会直接影响到我们的算法呈现您的内容并将其编入索引的效果，进而导致您的网站排名降低。

可以看到，禁止 JavaScript、CSS 文件的抓取不仅不会对 SEO 有任何正向的帮助，反而可能会起到降低网站排名的负面效果。

至于更具体的理由，谷歌在 [Understanding web pages better](https://webmasters.googleblog.com/2014/05/understanding-web-pages-better.html) （也就是上方引用的「以普通用户查看网页的方式来查看它」）中进行了进一步的解释，中心思想可以总结如下：

1.  时代已经变了，JavaScript、CSS 已经变成现代网页不可或缺的一部分了，它们让我们的网页变得更加丰富、更加美观，如果禁止 JS、CSS 被搜索引擎抓取，结果导致网页不能被更好地渲染，这对搜索者以及网站站长双方来说都是一种损失。
2.  为了避免这种双输的局面，我们（谷歌）为此做了很多工作，现在我们（谷歌）的抓取系统已经能够**以普通用户查看网页的方式来查看它**，而 JavaScript、CSS 代码是普通用户浏览网页时非常重要的组成的部分，所以不要再屏蔽 JavaScript、CSS 文件了。

上面这些都是谷歌在 2015 年就开始提倡的做法，所以现在已经 2020 年了，我们完全可以放心地让搜索引擎抓取我们的 JavaScript、CSS 文件，这样做对 SEO 有百利而无一害。

## 我的 robots.txt

在博客根目录下的 `public` 文件夹内新建 `robots.txt` 文件，然后 `hexo d` 部署网站即可。

```plaintext
# 允许任何搜索引擎抓取任何页面
User-agent: *
Allow: /
# 站点地图地址
Sitemap: https://asurada.zone/sitemap.xml
Sitemap: https://asurada.zone/baidusitemap.xml
```

## 参考链接

[谷歌官方搜索引擎优化 (SEO) 新手指南](https://support.google.com/webmasters/answer/7451184)

[Understanding web pages better](https://webmasters.googleblog.com/2014/05/understanding-web-pages-better.html)

[Don’t block CSS and JS files](https://yoast.com/dont-block-css-and-js-files/)
