---
title: "Hexo Next 主题 V7 升级 V8 记录"
source: https://asurada.zone/post/Update-Next-From-V7-To-V8/
date: 2020-09-02
updated: 2020-09-02
tags: [Hexo, Next]
categories: [个人博客]
---

# Hexo Next 主题 V7 升级 V8 记录

Hexo 和 Next 最近都发布了大版本更新，此文记录了升级过程。

## 升级 Hexo 版本

Hexo 核心团队成员 Sukka 写的 [Hexo 5.0.0 正式发布](https://blog.skk.moe/post/hexo-5/)详细介绍了 Hexo 5.0 的新特性，以及从 Hexo 4.x 升级到 Hexo 5.x 需要注意的地方。

Hexo 5.0 版本带来了不小的性能上的提升，升级 5.0 直接修改 `hexo-site/package.json` 文件：

```diff
"dependencies": {
-  "hexo": "^4.2.1",
+  "hexo": "^5.1.1",
```

然后再执行 `npm upadte` 命令即可。

升级完 Hexo ，`_config.yml` 并不会更新，而 Hexo 5.0 是有新增和改动一些其中的配置项的，所以需要我们自己去 [GitHub](https://github.com/hexojs/hexo-starter/blob/master/_config.yml) 上查看站点配置文件的变动情况，并手动同步这些修改到自己的站点配置文件里。

主要的变动就是新增了 `prismjs` 相关配置项以及更新了 `updated_option` 配置项。

## 升级 Next V8.0.0 版本

使用 Hexo 5.0 及以上版本，升级 Next 主题最简单的方法就是通过 `npm` 安装，进入博客根目录，执行以下命令即可安装最新版本的 Next 主题：

```autoit
npm install hexo-theme-next
```

通过 `npm` 安装的 Next 主题默认文件夹路径为 `hexo-site/node_modules/hexo-theme-next` 。因为有一些自定义修改可能会需要我们改动主题的源文件，而因为整个 `node_modules/` 被`gitignore` 文件所忽略，将会导致我们无法用 Git 记录我们对主题源文件的修改。

所以我将之前版本的 Next 主题文件夹进行改名，将 `hexo-site/theme/next` 改成了 `hexo-site/theme/next-V7`。然后将 `hexo-theme-next` 文件夹的内容复制到 `hexo-site/theme/next` 。这样就能将主题文件夹加入 Git 的管理之中了。

这里切记不要直接将旧版本的 Next 主题文件夹直接删除，因为后面可能有配置以及文件需要进行迁移。

更名完成后，使用 `hexo version` 查看版本号，可以看到一个硕大的 `Next` Logo 以及下方显示 `NexT version 8.0.0`，说明现在已经成功启用 Next V8 版本。

![查看 Next 版本号](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E6%9F%A5%E7%9C%8BNext%E7%89%88%E6%9C%AC%E5%8F%B7.png)

## 迁移配置文件

在之前使用 Next V7 版本的时候，我是利用 Hexo 的 Data Files 功能将主题配置放置在站点的 `source/_data/next.yml` 文件中。但这种做法在 V8 版本被官方告知已经过时，并**将在一段时间后停止支持**，现在推荐的做法是[使用 Alternate Theme Config 存储配置](https://theme-next.js.org/docs/getting-started/configuration.html)，需要 Hexo 版本在 5.0 及以上。

这里可以选择一步到位直接将主题配置整合到站点配置文件 `_config.yml` 中去，也可以选择依然使用独立的主题配置文件，我选择的是第二种。具体流程如下：

### 创建并配置 `_config.next.yml` 文件

在博客根目录下新建 `_config.next.yml` 文件。

因为 Next V8 有不少配置项发生了变更，所以我索性选择了重新配置。先将 `hexo-site/node_modules/hexo-theme-next/_config.yml` 中的内容复制到 `_config.next.yml` 中。然后用 Beyond Compare 软件对新主题配置文件 `_config.next.yml` 和旧主题配置文件 `source/_data/next.yml` 两者进行文本比较，根据自己的需求迁移整合相关配置项。

修改完成后，将旧主题配置文件 `source/_data/next.yml` 改名，我是改成了 `next-old.yml`，名字随意，只要能同原来的 `next.yml` 区别开来即可。

从 V7 升级到 V8 主要有下面几个变化：

-   Hexo 5.0 版本移除了对于 swig 模版的支持，使用 Nunjucks 代替 swig 作为模版引擎，主题配置文件内 `custom_file_path` 支持文件的后缀也从 `.swig` 变成了 `.njk`。所以如果此前根据 swig 的语法写过自定义内容，请在更新前确认它们是与 Nunjucks 兼容的，否则会报错，且生成的页面为空白。例如， Nunjucks 只支持 `and` 运算符，需要替换掉 swig 中的 `&&`。
-   Next V8 将自带的 Font Awesome 图标库由 4.7.0 版本升级为了 5.13.0 版本。此次升级并不向下兼容，所以需要修改配置文件中与 Font Awesome 相关的内容，否则图标可能无法正常显示。
-   NexT V8 已支持一键切换 CDN 服务商，而不需要为每个插件单独设置 CDN 地址。但需要注意，如果是使用最新版的 Next，那么一定要保持 `internal: local` 的默认设置不变。
-   如果此前配置过 Valine，需要注意将主题配置文件中的 `appid` 和 `appkey` 改为 `appId` 和 `appKey` 。否则会出现 Valine 无法正常加载的现象。
-   由于 Velocity.js 缺乏维护，NexT V8 使用 Animate.css 代替之。两者的动画效果几乎完全一致，除了动画名称略有不同。如果在配置文件中设置了旧的名称（例如 slideDownIn），请将其移除或根据此网页选择新的动画效果：[http://theme-next.js.org/animate/](http://theme-next.js.org/animate/)

更多详细信息，可以自行查询 Next [相关官方说明](https://github.com/next-theme/hexo-theme-next/issues/4)以及[官方文档](https://theme-next.js.org/docs/)。

### 迁移 Favicon 位置

之前我博客的 Favicon 图片啥的都是放在 `hexo-site/themes/next/source/images` 目录下面，正好趁着这次升级的机会，将图片全部移到了 `hexo-site/source/images` 下（没有这个文件夹就自己新建）。

不要忘记去 `_config.next.yml` 检查下 Favicon 相关配置的文件名是否正确，默认的都带有 `-next` 后缀。

## 自定义修改迁移

这一部分是最麻烦的，因为跨版本升级，很多源文件的位置以及名称都发生了变化，比如每篇文章结尾处的版权声明的样式文件就从 `post-copyright.styl` 变成了 `post-footer.styl`。而且模版引擎也从swig 变更成了 Nunjucks，虽然大部分内容是可以互相兼容的，但万一碰到不兼容的，排查起来就比较麻烦了。

所幸我的自定义修改并不多，而且都是 Nunjucks 兼容的，所以迁移起来相对较快。在进行迁移的时候，利用类似 Beyond Compare 之类的文件比较软件可以有效提高迁移速度，推荐大家尝试。

## 总结

折腾了一天，成功从 Next V7 升级到了 V8，升级所带来肉眼可见的变化并不多，只有一些诸如赞赏按钮样式之类的小变动，其他更多都是内部的一些变动。

我是喜欢自己折腾，而且及时使用新版，可以帮助发现并反馈问题，可以帮助 Next 主题更好地进行迭代。

## 参考链接

[hexo-theme-next README.md](https://github.com/next-theme/hexo-theme-next)

[Next 官方文档](https://theme-next.js.org/docs/)

[Next【必读】更新说明及常见问题](https://github.com/next-theme/hexo-theme-next/issues/4)

[Hexo 5.0.0 正式发布](https://blog.skk.moe/post/hexo-5/)
