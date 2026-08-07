---
title: "Hexo 博客主流搜索引擎收录详细指南"
source: https://asurada.zone/post/Blog-Search-Engine-Index/
date: 2020-08-28
updated: 2020-08-28
tags: [Hexo, SEO]
categories: [个人博客]
---

# Hexo 博客主流搜索引擎收录详细指南

如果希望自己博客的内容被更多人看到，搜索引擎所带来的流量是不容忽视的。本文详细介绍了如何向各大主流搜索引擎主动提交自己的博客，期望达到加快收录速度的目的。

## 生成站点地图

> 什么是站点地图？
> 
> 站点地图是一种文件，您可以在其中提供与您网站中的网页、视频或其他文件有关的信息，还可以说明这些内容之间的关系。Google 等搜索引擎会读取此文件，以便更加智能地抓取您的网站。站点地图会告诉 Google 您认为网站中的哪些网页和文件比较重要，还会提供与这些文件有关的重要信息：以网页为例，这些信息包括网页上次更新的时间、网页更改的频率，以及网页是否有其他语言版本。
> 
> *[谷歌帮助-了解站点地图](https://support.google.com/webmasters/answer/156184?hl=zh-Hans&ref_topic=4581190)*

安装 [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap) 插件：

```shell
npm install hexo-generator-sitemap --save
```

安装插件以后，执行 `hexo g` 命令，即可在 `public` 文件夹下找到新生成的 `sitemap.xml` 文件。

## 让百度收录站点

### 百度验证域名

刚建站的时候，百度是没有收录我们的网站的，这时在搜索框输入 `site:<域名>` ，结果应该是下面这样的，点击「提交网址」来提交我们的站点。

![百度未收录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/1-%E7%99%BE%E5%BA%A6%E6%9C%AA%E6%94%B6%E5%BD%95.png)

点击后会进入「[百度搜索资源平台](https://ziyuan.baidu.com/site)」，用自己的百度账号进行登录。登录后「站点管理」–> 「添加站点」–> 「添加网站」。

![百度添加网站](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/7-%E7%99%BE%E5%BA%A6%E6%B7%BB%E5%8A%A0%E7%AB%99%E7%82%B9%E8%B7%AF%E5%BE%84.png)

百度添加网站是需要进行实名认证的，这一点需要注意。

![百度实名](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/8-%E7%99%BE%E5%BA%A6%E9%9C%80%E8%A6%81%E5%AE%9E%E5%90%8D.png)

填完个人信息后，提交站点网址，选择站点属性，最后一步验证网站。

![选择站点属性](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/9-%E7%99%BE%E5%BA%A6%E8%AE%BE%E7%BD%AE%E7%AB%99%E7%82%B9%E5%B1%9E%E6%80%A7.png)

验证方式选择第二种「HTML标签验证」，复制下图 `content` 里的字符串。

![选择验证方式](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/10-%E7%99%BE%E5%BA%A6%E9%80%89%E6%8B%A9%E9%AA%8C%E8%AF%81%E6%96%B9%E5%BC%8F.png)

> 此项配置基于 Hexo 4.2，Next 7.8.0

打开主题配置数据文件 `next.yml` ，搜索 `baidu_site_verification`，将上述字符串填至后方。

```yaml
# Baidu Webmaster tools verification.
# See: https://ziyuan.baidu.com/site
baidu_site_verification: hpYpGBuDQF
```

执行 `hexo g -d` 重新生成并部署站点，部署完成后，检查网站源码，能看到在`<head>` 里面已经自动包含了用于验证身份的 meta 标签：

![百度验证 meta 标签](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/11-%E7%99%BE%E5%BA%A6%E9%AA%8C%E8%AF%81meta%E6%A0%87%E7%AD%BE.png)

回到百度站长管理平台点击「完成验证」按钮，完成验证。

![百度验证成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/12-%E7%99%BE%E5%BA%A6%E9%AA%8C%E8%AF%81%E6%88%90%E5%8A%9F.png)

当我验证完成的时候，百度通知我说 HTTPS 认证失败，当时**我没有进行任何处理**，二个小时后我手动在「搜索展现」–> 「HTTPS 认证」申请认证，几分钟后就直接显示「认证通过」了，所以认证失败了不要慌，可能等待一会就自然好了。

![百度Https认证成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/13-%E7%99%BE%E5%BA%A6Https%E8%AE%A4%E8%AF%81%E6%88%90%E5%8A%9F.png)

### 百度提交链接

百度提交链接有下面三种方式。

#### 主动提交

安装 [hexo-baidu-url-submit](https://github.com/huiwang/hexo-baidu-url-submit) 插件：

```shell
npm install hexo-baidu-url-submit --save
```

获取百度主动推送`准入密钥`并复制：

![百度主动推送准入密钥](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/14-%E7%99%BE%E5%BA%A6%E4%B8%BB%E5%8A%A8%E6%8E%A8%E9%80%81%E5%87%86%E5%85%A5%E5%AF%86%E9%92%A5.png)

在站点配置文件 `_config.yml` 中**新增**以下代码：

```diff
+ # 百度主动推送
+ baidu_url_submit:
+ 	count: 5 # 提交最新的五个链接
+ 	host: https://asurada.zone # 在百度站长平台中注册的域名
+  	token: xxxxx  # 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
+  	path: baidu_urls.txt  # 文本文档的地址， 新链接会保存在此文本文档里
```

然后在站点配置文件 `_config.yml` 中修改部署策略：

```diff
  # Deployment
  ## Docs: https://hexo.io/docs/deployment.html
- deploy:
- 	type: git
-   repo: git@github.com:ousugo/ousugo.github.io.git
-	branch: master
+ deploy:
+ - type: git
+   repo: git@github.com:ousugo/ousugo.github.io.git
+	branch: master
+ - type: baidu_url_submitter # 百度
```

此后每次执行 `hexo d` 部署博客的时候将会自动在 `./public` 目录下生成 `baidu_urls.txt` 文件，里面的内容就是自己设置的最新的 n 条文章链接。然后自动推送文件内容到百度，加快链接被百度收录的速度。

在这里的时候我踩了一个坑，在部署的时候一直提示错误：`{"error":401,"message":"site sid is empty"}` ，后面经过排查发现是在配置主动推送时，`host` 后面的网址没有加协议头，加上之后就不报错了。

```diff
baidu_url_submit:
  count: 5 # 提交最新的五个链接
- host: asurada.zone # 在百度站长平台中注册的域名
+ host: https://asurada.zone # 在百度站长平台中注册的域名	
  token: xxxxx  # 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt  # 文本文档的地址， 新链接会保存在此文本文档里
```

#### ~自动推送~（弃用）

百度站长平台现在已经取消了「自动推送」的功能入口，所以不建议再开启此功能，详见[百度 JS代码推送功能已停止使用，删除推送代码吧](https://51.ruyo.net/16280.html)。

> 此项配置基于 Hexo 4.2，Next 7.8.0

Next 主题中已经内置了一键开启百度自动推送的选项：

```yaml
# Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO
baidu_push: true
```

开启后将会自动在页面中添加如下脚本用于百度推送：

![推送脚本代码](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/18-%E7%99%BE%E5%BA%A6JS%E6%8E%A8%E9%80%81%E8%84%9A%E6%9C%AC%E4%BB%A3%E7%A0%81.png)

之后每次访问站点页面都会通过以上脚本将当前页面的 URL 推送到百度。

#### sitemap

博客如果是部署在 GitHub Pages 上，baidusitemap 是无法被成功抓取的，因为 GitHub 禁止了百度爬虫，最常见的解决办法是将博客双线部署到 Coding Pages 和 GitHub Pages，因为百度爬虫可以爬取到 Coding 上的内容，从而实现百度收录。实现双线部署可以参考我的这篇 [Hexo 博客 GitHub Pages + CODING 静态网站双部署](https://asurada.zone/post/Deploy-Hexoblog-To-Codingnet-And-GitHub/) 博客。

安装 [hexo-generator-baidu-sitemap](https://github.com/coneycode/hexo-generator-baidu-sitemap) 插件：

```mipsasm
npm install hexo-generator-baidu-sitemap --save
```

插件安装成功后，执行 `hexo g -d` 部署网站，然后访问 `网站自定义域名/baidusitemap.xml` 检查百度网站地图是否正常。比如我的是 `asurada.zone/baidusitemap.xml` 。

然后来到百度站长平台的 [sitemap 提交页面](https://ziyuan.baidu.com/linksubmit/)，提交 sitemap 地址即可，注意需要自己添加 `https://` 协议头，不然默认是 `http://`，会导致抓取失败。

![百度提交 sitemap](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/20-%E7%99%BE%E5%BA%A6%E6%8F%90%E4%BA%A4sitemap.png)

初次提交要等几分钟，如果成功的话状态会显示为正常，并显示成功提取 URL 的数量。如果以后博客有新的文章或其他页面，可以点击**手动更新文件**，对 sitemap 进行更新。

![百度 sitemap 抓取正常](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/21-%E7%99%BE%E5%BA%A6sitemap%E6%8A%93%E5%8F%96%E6%AD%A3%E5%B8%B8.png)

百度的收录速度是比较慢的，一两周都是很正常的。

## 让谷歌收录站点

### GSC 验证域名

登陆 [Google Search Console](https://search.google.com/u/1/search-console/welcome) （后面简称 GSC)，可以看到有两种资源类型，因为目前我博客的域名是没有任何前缀的裸域名，所以我选择「网域验证」。

![Google Search Console 欢迎页](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/2-GoogleSearchConsole%E6%AC%A2%E8%BF%8E%E9%A1%B5.png)

要进行「网域验证」的话，只有「通过 DNS 记录来验证域名所有权」这一种办法。复制这一步中出现的 TXT 记录值，然后进到域名解析服务商的管理界面，比如我的是 DNSPod。

![DNS 验证域名所有权](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/4-DNS%E9%AA%8C%E8%AF%81%E5%9F%9F%E5%90%8D%E6%89%80%E6%9C%89%E6%9D%83.png)

在「记录管理」里面新增一条 `TXT` 类型的解析记录，主机记录留空或者填 `@`，记录值就是刚刚复制的那一串记录值，其他保持默认即可。

![DNSPod 新增谷歌域名验证解析记录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/32-DNSPod%E6%96%B0%E5%A2%9E%E8%B0%B7%E6%AD%8C%E5%9F%9F%E5%90%8D%E9%AA%8C%E8%AF%81%E8%A7%A3%E6%9E%90%E8%AE%B0%E5%BD%95.png)

稍等片刻，等待 DNS 解析生效，然后回到 GSC 点击「验证」按钮，即可完成验证。

![GSC 网域验证成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/3-Google%E7%BD%91%E5%9F%9F%E9%AA%8C%E8%AF%81%E6%88%90%E5%8A%9F.png)

### 提交站点地图

验证域名所有权之后，进入 GSC 后台管理界面，输入 sitemap 地址并点击提交即可。谷歌的收录是比较快速的，两天左右就能够在谷歌通过 `site:博客网址` 的方式检索到收录结果了。

![GSC 提交站点地图](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/33-GSC%E6%8F%90%E4%BA%A4%E7%AB%99%E7%82%B9%E5%9C%B0%E5%9B%BE.png)

## 让 360 搜索收录网站

### 注册 360 站长平台

注册并登录[360 站长平台](https://asurada.zone/post/Blog-Search-Engine-Index/zhanzhang.so.com/)，添加网站。

![360 站长平台添加网站](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/22-360%E7%AB%99%E9%95%BF%E5%B9%B3%E5%8F%B0%E6%B7%BB%E5%8A%A0%E7%BD%91%E7%AB%99.png)

添加网站后需要进行验证，选择「代码验证」，复制 `content` 后的字符串，后面会用到。

![代码验证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/23-360%E7%BD%91%E7%AB%99%E4%BB%A3%E7%A0%81%E9%AA%8C%E8%AF%81.png)

### 360 验证域名

修改站点配置文件 `_config.yml` ：

```diff
# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
- #head: source/_data/head.swig
+ head: source/_data/head.swig
  #header: source/_data/header.swig
```

然后在 `博客根目录\source\_data` 下新建 `head.swig` 文件，然后打开并新增以下内容：

```plaintext
{% if theme.so_site_verification %}
  <meta name="360-site-verification" content="{{ theme.so_site_verification }}"/>
{% endif %}
```

上面代码是利用了 Next 主题的 [Inject 功能](https://theme-next.js.org/docs/advanced-settings.html#Injects)，以上代码的含义为如果在 `_config.yml` 中存在 `so_site_verification` 这一设置项，则会将 `<meta name="360-site-verification" content="so_site_verification 设置项的 value"/>`插入到网站首页 HTML 代码的 `<head></head>` 标签中。

再次修改站点配置文件 `_config.yml`，在 `SEO Settings`区域新增以下内容 ：

```avrasm
# 360 Webmaster tools verification.
# See: zhanzhang.so.com/sitetool
so_site_verification: 此处填写之前复制的验证字符串
```

修改完成后，执行 `hexo g -d` 命令重新生成并部署网站，部署生效后，回到 360 站长平台，「点击验证」即可。如果网站已经开启了 HTTPS 访问，不要忘记「设置 https」。

![360 站长平台验证成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/24-360%E7%AB%99%E9%95%BF%E5%B9%B3%E5%8F%B0%E9%AA%8C%E8%AF%81%E6%88%90%E5%8A%9F.png)

### 提交 sitemap

360 站长平台体验很一般，连 HTTPS 都没上，然后在进行 sitemap 提交的时候，第一天页面就一直报「连接超时」，完全无法提交。第二天终于不报错了，可以正常提交了，页面也显示成功提交，但添加完以后 sitemap 列表却一直是空的，让人很困惑，不知道这到底是添加成功了但还没爬取呢还是压根就没添加上。终于第三天一切顺利，成功添加并显示了。

我一开始提交的是**只有裸域名**的 sitemap 地址，但是几天后发现无法抓取到任何链接，于是我尝试在域名前加上 `www`，没想到还真有效果，很快就把全部链接都给抓取到了。具体的原因目前我也没有弄清楚。

![360 站长平台提交sitemap](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/6-360%E6%8F%90%E4%BA%A4sitemap.png)

## 让搜狗收录网站

### 注册搜狗站长平台

注册并登录[搜狗站长平台](http://zhanzhang.sogou.com/)，我在这里很想吐槽，这都 2020 年了，360 和搜狗的站长平台居然**都还没上 HTTPS**，我一度怀疑自己是不是进了伪造网站。这样给我的感觉就是完全没有用心在做搜索。

搜狗的添加网站也和别家不一样，很奇葩地单独设置在「用户中心」里，体验上很割裂。

![搜狗站长平台添加网站](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/27-%E6%90%9C%E7%8B%97%E7%AB%99%E9%95%BF%E5%B9%B3%E5%8F%B0%E6%B7%BB%E5%8A%A0%E7%BD%91%E7%AB%99.png)

### 搜狗验证域名

填加网站后同样需要验证，这次我们图省事，直接选择「文件验证」，点击「下载验证文件」，将 `sogousiteverification.txt` 下载到 `博客根目录\public` 文件夹里面，然后直接执行 `hexo d` 命令部署网站，部署生效后，回到搜狗站长平台「完成验证」。

![搜狗域名文件验证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/28-%E6%90%9C%E7%8B%97%E5%9F%9F%E5%90%8D%E6%96%87%E4%BB%B6%E9%AA%8C%E8%AF%81.png)

### 提交链接

搜狗站长平台居然不支持 sitemap 提交，说是采取邀请制，这波操作反正我是看不懂。

![搜狗不支持 sitemap](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/30-%E6%90%9C%E7%8B%97%E4%B8%8D%E6%94%AF%E6%8C%81sitemap.png)

那只能退而求其次，手动提交链接了，得亏是我现在文章还比较少。不过倒可以等文章更多一些，试着申请一下开通 sitemap 提交权限。

![搜狗提交链接](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/31-%E6%90%9C%E7%8B%97%E6%8F%90%E4%BA%A4%E9%93%BE%E6%8E%A5.png)

## 让 Bing 搜索收录网站

Bing 我之前并没有申请过收录，但是在使用 `site:asurada.zone` 检查收录情况时，居然惊喜地发现已经收录了我的博客，虽然仅仅只有一条链接。

### 直接从 Google Search Console 导入网站

打开[Bing Webmaster](https://www.bing.com/webmaster)，使用绑定了 Google Search Console（后面简称 GSC） 的谷歌账号登录，这样后面就可以很方便地直接从 GSC 直接导入网站，而不需要再单独验证网站。

![ GSC 直接导入](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/25-BingWebmasterGSC%E7%9B%B4%E6%8E%A5%E5%AF%BC%E5%85%A5.png)

### 提交网站地图

按理说应该可以直接从 GSC 获取网站地图，但我的并没有获取到，不管了，手动提交也很简单。Bing 处理 sitemap 的时间大概在一天左右。

![Bing 提交网站地图](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/26-Bing%E6%8F%90%E4%BA%A4%E7%BD%91%E7%AB%99%E5%9C%B0%E5%9B%BE.png)

## 让 Yandex 搜索收录网站

### 注册 Yandex Webmaster

注册并登录[Yandex Webmaster](https://asurada.zone/post/Blog-Search-Engine-Index/webmaster.yandex.com)，这里**有一个坑需要注意**，Yandex 目前是可以直连的，但是在注册账号的时候，你会发现在最后一步输入验证码的时候，不管你怎么填总会提示验证码错误。而且 Yandex 的验证码是很奇葩的两张图片，让我弄不清楚是该输其中一张图片的内容还是怎么着，折腾了半天。最后在网上查到的解决方法是**不要直连，而是通过科学上网**进行注册，这时再输验证码就不会报错了，无语，验证码是两张图片的内容都要输入。

![Yandex Webmaster](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/35-YandexWebmaster.png)

### Yandex 验证域名

> 此项配置基于 Hexo 5.0.2，Next 7.8.0

进入 Webmaster 后台管理界面添加域名，这里**需要注意的是**和在其他站长平台添加网址不一样，Yandex 在添加网址时就需要写清楚**带不带 `www`**，是使用 `http` 还是 `https`，**默认是使用 `http` 的**。

![Yandex 添加网址注意事项](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/7-Yandex%E6%B7%BB%E5%8A%A0%E5%9F%9F%E5%90%8D%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9.png)

选择域名验证方式，因为 Next 主题直接内置 Yandex 的验证，所以我们选择「Meta tag」验证，复制 `content` 后双引号内的字符串。

![Yandex 添加网站](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/34-Yandex%E6%B7%BB%E5%8A%A0%E7%BD%91%E7%AB%99.png)

打开 Next 主题配置文件 `next.yml`，搜索关键字 ”Yandex”，找到`yandex_site_verification`，填入刚刚复制的字符串。

```diff
# Yandex Webmaster tools verification.
# See: https://webmaster.yandex.ru
- yandex_site_verification:
+ yandex_site_verification: 4aca0e5ccd3fbc2f
```

修改完成后，执行 `hexo g -d` 命令重新生成并部署网站，部署生效后，回到 Yandex Webmaster，点击 “Check” 即可。

![Yandex 完成验证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/36-Yandex%E5%AE%8C%E6%88%90%E9%AA%8C%E8%AF%81.png)

### 提交 sitemap

验证完成后，在左侧菜单「Indexing」–> 「Sitemap files」中提交 sitemap ，然后等待两天时间即可完成 sitemap 的收录和牵引。

![Yandex Sitemap 收录成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/9-YandexSitemap%E6%94%B6%E5%BD%95%E6%88%90%E5%8A%9F.png)

之后就可以在左侧菜单「Indexing」–> 「Searchable pages」中查看网站的哪些页面已经被 Yandex 搜索引擎所收录。

## 让头条搜索收录网站

### 注册头条站长平台

注册并登录[头条搜索站长平台](https://asurada.zone/post/Blog-Search-Engine-Index/zhanzhang.toutiao.com)，添加网站。

![头条站长平台](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/1-%E5%A4%B4%E6%9D%A1%E7%AB%99%E9%95%BF%E5%B9%B3%E5%8F%B0.png)

### 头条验证网站

头条现在的验证方式非常单一，只能通过「文件校验」的方式来验证。直接下载验证文件并将其移动到博客根目录下的 `public` 文件夹里面。然后执行 `hexo d` 命令部署网站，部署生效后，回到站长平台完成验证。

![头条网站文件验证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/2-%E5%A4%B4%E6%9D%A1%E7%BD%91%E7%AB%99%E9%AA%8C%E8%AF%81.png)

### 提交 sitemap

验证完毕后，提交 sitemap。

![头条提交 sitemap](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/3-%E5%A4%B4%E6%9D%A1%E6%8F%90%E4%BA%A4sitemap.png)

提交完成后，即可在页面下方查看 sitemap 的更新情况：

![头条 sitemap 收录情况](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/8-%E5%A4%B4%E6%9D%A1sitemap%E6%94%B6%E5%BD%95%E6%83%85%E5%86%B5.png)

## 让神马搜索收录网站

### 注册神马站长平台

注册并登录[神马搜索站长平台](https://asurada.zone/post/Blog-Search-Engine-Index/zhanzhang.sm.cn)，添加网站。

![神马站长平台](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/4-%E7%A5%9E%E9%A9%AC%E7%AB%99%E9%95%BF%E5%B9%B3%E5%8F%B0.png)

### 验证网站

直接下载验证文件 `shenma-site-verification.txt` 并将其移动到博客根目录下的 `public` 文件夹里面。然后执行 `hexo d` 命令部署网站，部署生效后，回到站长平台完成验证。

![神马网站文件验证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/5-%E7%A5%9E%E9%A9%AC%E7%BD%91%E7%AB%99%E6%96%87%E4%BB%B6%E9%AA%8C%E8%AF%81.png)

### 放弃神马收录

神马在验证网站时，不管是文件验证还是代码验证，我多次尝试都以失败告终，使用体验极差，遂弃之。

## 总结

各大搜索引擎的收录步骤都是大同小异的，没啥太大的难度，一步步来就可以了。

整体体验下来，感觉搜狗、神马压根就没想把自己的搜索给做起来了，体验一塌糊涂。360 体验稍好一点，不过作为国内搜索的老二，站长平台连个 HTTPS 都不上，属实说不过去。相较于国内参差不齐的体验，国外主流搜索引擎的站长平台的各种体验都要好上不少，人家强是有道理的。

最后想说的，也是对自己的勉励，那就是「**内容才是第一要素**」。SEO 技巧是为内容服务的，收录只是第一步，收录以后还需要持续更新博客，写有价值、有内容的文章，这样才能吸引他人点击和浏览，乃至分享引用，这样可以增加网站曝光以及提高搜索引擎权重，从而得到更好的搜索引擎排名，让更多的人看到，实现正向循环。

## 参考链接

[Hexo-NexT 博客提交搜索引擎收录](https://tding.top/archives/4ea54752.html)

[Hexo 博客提交百度、谷歌搜索引擎收录](https://www.itrhx.com/2019/09/17/A48-submit-search-engine-inclusion/)
