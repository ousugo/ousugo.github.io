---
title: "Hexo 博客修改记录"
source: https://asurada.zone/post/hexo-blog-settings/
date: 2020-05-08
updated: 2020-05-08
tags: [Hexo]
categories: [个人博客]
---

# Hexo 博客修改记录

记录自己博客所做的修改，供他人参考以及自己备忘。

## 提交搜索引擎收录

博客搭建好了，如果希望自己博客的内容被更多人看到，搜索引擎所带来的流量是不容忽视的。为了让搜索引擎更快地收录我们的网站，我们可以主动到搜索引擎的站长平台提交我们的网站。

因为搜索引擎收录的内容比较多，所以专门单独写了一篇文章，具体内容请跳转 [Hexo 博客主流搜索引擎收录详细指南](https://asurada.zone/post/Blog-Search-Engine-Index/)查看。

## 站内搜索

> 此项配置基于 Hexo 5.1.1，Next 8.0.0，hexo-generator-searchdb V1.3.2

该功能由 [hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb) 提供，效果如下图：

![站内搜索](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/37-%E7%AB%99%E5%86%85%E6%90%9C%E7%B4%A2%E6%95%88%E6%9E%9C%E5%9B%BE.png)

在根目录下执行以下命令安装插件：

```cmake
$ npm install hexo-generator-searchdb
```

在**主题配置**文件中修改相关字段：

```plaintext
local_search:
  enable: true
  trigger: auto # 每次输入改变都重新搜索
  top_n_per_article: 3 # 每篇文章显示的搜索结果数量
  unescape: false
```

在**站点配置**文件中**添加**以下字段：

```plaintext
search:
  path: search.xml # 后缀可选 .xml 和 .json
  field: post # 指定搜索范围，可选 post | page | all
  content: true # 是否牵引文章的全部内容，false 则只牵引文章标题
  format: html # 指定页面内容形式，可选 html | raw (Markdown) | striptags 
```

配置完成后，执行 `hexo g` 命令会在 `public` 文件夹下自动生成 `search.xml` 文件。

## 文末版权声明修改

### 版权声明添加标题

> 此项配置基于 Hexo 4.2，Next 7.8.0

Next 主题自带的版权声明默认只有作者、链接、版权声明三项，为方便他人转载，我觉得有必要加入文章标题的显示。

Next 8.0 以上版本改为在 `post-copyright.njk` 中修改。

首先修改 `.\themes\next\layout\_partials\post\post-copyright.swig` 文件，在 `<ul> </ul>` 标签内根据自己的需求在相应位置**新增**以下代码：

```html
<li class="post-copyright-title">
    <strong>{{ __('post.copyright.title') + __('symbol.colon') }} </strong>
    {{ post.title }}
  </li>
```

由于 `post.copyright.title` 是我们自己新增的，这一字段 Next 默认是没有对应的中文翻译的，所以我们需要通过修改 `.\themes\next\languages\zh-CN.yml` 文件来添加翻译，打开文件，搜索 `copyright` ，在下面新增 `title: 本文标题` 。

![zh-CN.yml 增加对应翻译](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/5-zhCN%E5%A2%9E%E5%8A%A0%E7%BF%BB%E8%AF%91.png)

执行 `hexo g` 重新生成博客，`hexo s` 本地调试查看效果：

![本文标题效果图](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/6-%E6%9C%AC%E6%96%87%E6%A0%87%E9%A2%98%E6%95%88%E6%9E%9C%E5%9B%BE.png)

效果正是我想要的，确认无误后 `hexo d` 部署。

### 「本文链接」添加快速复制按钮

> 此项配置基于 Hexo 4.2，Next 7.8.0，本小节主要参考 [yearito’s Blog | Hexo搭建个人博客系列：进阶设置篇](http://yearito.cn/posts/hexo-advanced-settings.html)

Next 8.0 以上版本改为在 `post-copyright.njk` 中修改。

Next V8 将自带的 Font Awesome 图标库由 4.7.0 版本升级为了 5.13.0 版本。此次升级并不向下兼容，所以需要修改配置文件中与 Font Awesome 相关的内容，否则图标可能无法正常显示。

修改 `.\themes\next\layout\_partials\post\post-copyright.swig` 文件，改动如下：

```diff
{%- set ccIcon = '<i class="fa fa-fw fa-creative-commons"></i>' %}
{%- set ccText = theme.creative_commons.license | upper %}


+<!-- JS库 clipboard 用来拷贝内容到粘贴板-->
+<script src="https://cdn.jsdelivr.net/npm/clipboard@2.0.6/dist/clipboard.min.js"></script>

+<!-- JS库 sweetalert 用来显示提示信息-->
+<script src="https://cdn.jsdelivr.net/npm/sweetalert@2.1.2/dist/sweetalert.min.js"></script>

<div>
<ul class="post-copyright">

  <!-- 本文标题 -->
  <li class="post-copyright-title">
    <strong>{{ __('post.copyright.title') + __('symbol.colon') }} </strong>
    {{ post.title }}
  </li>

  <!-- 本文作者 -->
  <li class="post-copyright-author">
    <strong>{{ __('post.copyright.author') + __('symbol.colon') }} </strong>
    {{- page.author or author }}
  </li>

  <!-- 引用链接 -->
  <li class="post-copyright-link">
    <strong>{{ __('post.copyright.link') + __('symbol.colon') }}</strong>
    {{ next_url(page.permalink, page.permalink, {title: page.title}) }}
+    <span class="copy-path" title="点击复制 Markdown 引用链接"
+      ><i
+        style="cursor: pointer"
+        class="fa fa-clipboard"
+        data-clipboard-text="[{{ post.author | default(author) }}'s Blog | {{ post.title }}]({{ post.permalink }})"
+        aria-label="{{ __('post.copyright.copy_success') }}"
+      ></i
+    ></span>
+  </li>
    
  <!-- 版权声明 -->
  <li class="post-copyright-license">
    <strong>{{ __('post.copyright.license_title') + __('symbol.colon') }} </strong>
    {{- __('post.copyright.license_content', next_url(ccURL, ccIcon + ccText)) }}
  </li>
</ul>

+ <script>
+   var clipboard = new ClipboardJS(".fa-clipboard");
+  clipboard.on("success", function(target) {
+    var message = document.createElement("div");
+    message.innerHTML =
+      '<i class="fa fa-check-circle message-icon"></i><span class="message-content">' +
+      target.trigger.getAttribute("aria-label") +
+      "</span>";
+    swal({
+      content: message,
+      className: "copy-success-message",
+      timer: 1000,
+      button: false
+    });
+  });
+ </script>

</div>
```

Next 8.0 以上版本改为在 `post-footer.styl` 中修改。

在版权样式文件 `.themes\next\source\css\_common\components\post\post-copyright.styl` 中**新增**如下样式：

```css
.swal-overlay {
  background-color: transparent;
}

.copy-success-message {
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
  width: auto;
  margin: 16x 0px;
  vertical-align: middle;
}

.copy-success-message .swal-content {
  margin: 0px 0px !important;
  padding: 10px 16px;
  line-height: 1em;
}

.copy-success-message .message-icon {
  color: #52c41a;
  margin-right: 8px;
}

.copy-success-message .message-content {
  font-size: 14px;
}
```

然后补全版权信息文案字段：

```diff
post:
  copyright:
+   copy_success: 复制成功
```

之后点击「本文链接」后的图标便可以快捷复制 Markdown 格式的引用链接，并将弹出复制成功的提示语。

> 该功能主要借助 [clipboard](https://clipboardjs.com/) 和 [sweetalert](https://sweetalert.js.org/) 两个 js 库来实现，并参考 [antDesign | message 组件](https://ant.design/components/message/) 重写了弹框样式。

![链接快速复制效果](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/17-%E6%9C%AC%E6%96%87%E9%93%BE%E6%8E%A5%E5%BF%AB%E9%80%9F%E5%A4%8D%E5%88%B6%E6%95%88%E6%9E%9C.png)

## 修改文章 URL 地址

> 此项配置基于 Hexo 4.2，Next 7.8.0

Hexo 默认的文章 URL 地址为 `https://yoursite.com/:year/:month/:day/:title/`，层级较多，我自己看着碍眼，据说对 SEO 也不利。打开站点配置文件 `_config.yml` ，找到 `permalink`，将其改成自己想要的格式，我的很简单，全部的文章都是 `post`。

```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://asurada.zone
root: /
-permalink: :year/:month/:day/:title/
+permalink: post/:title/
permalink_defaults:
```

## 生成 RSS 并在侧边栏添加 RSS 按钮

> 此项配置基于 Hexo 5.0，Next 7.8.0

### 安装 hexo-generator-feed 插件

在 Hexo 根目录执行命令，安装 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 插件：

```shell
npm install hexo-generator-feed --save
```

安装结束时后，此时重新 `hexo g` 便会直接在 `Public` 文件夹下自动生成 `atom.xml`文件。默认使用 atom 格式、输出全文、日期倒序排列，如果预设符合自己的要求，可以直接跳过下一步。

### 配置站点配置文件

在站点配置文件 `_config.yml`添加相关配置：

```diff
+# RSS
+feed:
+ type: atom
+ path: atom.xml
+ limit: false
```

配置含义：

1.  `type`：RSS的类型，可选 atom/rss2，默认 atom
2.  `path`：Feed 路径，默认 ./atom.xml 或 ./rss2.xml
3.  `limit`: 展示文章的数量，使用 0 或 false 则输出所有文章

### 侧边栏添加 RSS 图标

Next 在[V7.6.0](https://github.com/theme-next/hexo-theme-next/releases/tag/v7.6.0)中移除了侧边栏的 RSS 图标，后续又在[V7.7.1](https://github.com/theme-next/hexo-theme-next/releases/tag/v7.7.1)中通过「Follow\_me」功能将 RSS 图标引入到了文章的结尾区域。但我觉得 RSS 在整个网站的优先级应该足够高，下面是如何将 RSS 图标重新加入侧边栏的方法。

原理其实很简单，就是利用 Next 主题自带的 Social Links 功能，将 RSS 也变成一个 Social Link。修改主题配置文件 `.\source\_data\next.yml` 如下：

```diff
social:
  GitHub: https://github.com/ousugo || github
+ RSS: https://asurada.zone/atom.xml || rss
  #E-Mail: mailto:yourname@gmail.com || envelope
```

“Soical Links” 的格式为 `Key: permalink || icon`，其中 `Key` 为图标旁边显示的名称，`permalink` 为目标地址，这里指 RSS Feed 的地址，`icon` 为将要显示的图标在 [Font Awesome](https://fontawesome.com/) 中的名字。

最终效果如下：

![侧边栏RSS图标效果](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/19-%E4%BE%A7%E8%BE%B9%E6%A0%8FRSS%E5%9B%BE%E6%A0%87%E6%95%88%E6%9E%9C.png)

## 美化

### 文末添加结束标记

> 此项配置基于 Hexo 4.2，Next 7.8.0

感觉文章结束以后，紧接着就是打赏按钮，显得较为突兀，所以在文末添加结束标记，效果如下：

![文末结束标记](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/15-%E6%96%87%E6%9C%AB%E7%BB%93%E6%9D%9F%E6%A0%87%E8%AE%B0.png)

修改主题配置文件 `.\source\_data\next.yml`，取消 `postBodyEnd` 前的注释，启用 `post-metabody-end.swig` 这一自定义文件：

```yaml
# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
- #postBodyEnd: source/_data/post-metabody-end.swig
+ postBodyEnd: source/_data/post-metabody-end.swig
```

我们可以打开 `.\themes\next\layout\_macro\post.swig` 文件，搜索 `next_inject`，定位到 `next_inject('postBodyEnd')`：

![post.swig 插入位置](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/16-post_swig%E6%8F%92%E5%85%A5%E4%BD%8D%E7%BD%AE.png)

这一段代码的含义就是当上面的 `postBodyEnd` 被取消注释时，`post-metabody-end.swig` 文件就会被插入到这里（望文生义，如理解有误请指出）。

在 `.\source\_data` 文件下新建 `post-metabody-end.swig` 文件，添加如下代码：

```html
<div>
    <div style="text-align:center;color:#bfbfbf;font-size:16px;">
      <span>-------- 本文结束 </span>
      <i class="fa fa-paw"></i>		<!-- 这里的图标可以自己去 fontawesome.com 找，然后自定义 -->
      <span> 感谢阅读 --------</span>
    </div>
</div>
```

`hexo s -g` 预览效果没问题，就可以 `hexo d` 部署到博客了。

## 参考链接

[yearito’s Blog | Hexo搭建个人博客系列：部署上线篇](http://yearito.cn/posts/hexo-deploy-to-VPS.html)

[小丁的个人博客 | Hexo-NexT 博客提交搜索引擎收录](https://tding.top/archives/4ea54752.html)

[yearito’s Blog | Hexo搭建个人博客系列：主题美化篇](http://yearito.cn/posts/hexo-theme-beautify.html)

[yearito’s Blog | Hexo搭建个人博客系列：进阶设置篇](http://yearito.cn/posts/hexo-advanced-settings.html)
