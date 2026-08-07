---
title: "Hexo 博客 GitHub Pages + CODING 静态网站双部署"
source: https://asurada.zone/post/Deploy-Hexoblog-To-Codingnet-And-GitHub/
date: 2020-08-12
updated: 2020-08-12
tags: [Hexo, GitHub, DNS]
categories: [个人博客]
---

# Hexo 博客 GitHub Pages + CODING 静态网站双部署

## 为什么想要双部署

中文博客的受众肯定是以国内为主，但部署在 Github Pages 上，服务器在国外，国内访问的速度太不稳定，如果一个页面需要将近十秒才能完全打开的话，用户体验差，页面被提前关闭的概率也会呈指数级上升。所以改善国内访问体验迫在眉睫。

之前也想过直接使用 CDN 直接加速 GitHub Pages 的方式，但国内的 CDN 全部要求域名进行备案，嫌麻烦就放弃了。最后选择通过将博客同时部署到 GitHub 和 CODING 的方式来提升国内访问体验。

CODING 在 19 年年底进行了一次比较大的改版，双部署的整个流程也有了不小的变化，而网络上很多教程大部分都还是基于旧版流程的。本文详细记载了将博客同时部署到 GitHub Pages 和 CODING 静态网站服务的全流程。

## CODING 配置

### 注册 CODING 账号

2019 年 12 月 28 日以后，CODING 统一了入口，不再区分个人版与否，具体的升级详情可以查看[官方通知](https://coding.net/products/upgrade)。升级后的 CODING 会增加「团队/组织」的管理概念，注册的时候必须先创建一个团队，这个和平常的注册稍有些不同，需要注意一下。[点击跳转 CODING 官网](https://coding.net/)。

![CODING 注册界面](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/Coding%E6%B3%A8%E5%86%8C%E7%95%8C%E9%9D%A2.png)

### 创建 CODING 项目

注册成功以后，进入「项目」–> 「创建项目」。

![CODING 创建项目](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CODING%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE.png)

选择「代码托管项目」，只需填写项目名称，其他都保持空白和默认即可。注意**不要勾选**「公开源代码」。

![创建代码托管项目](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E5%88%9B%E5%BB%BA%E4%BB%A3%E7%A0%81%E6%89%98%E7%AE%A1%E9%A1%B9%E7%9B%AE.png)

### CODING 配置 SSH 公钥

SSH 密钥设置的路径为 「右上角个人头像」–>「个人账户设置」–> 「SSH 公钥」–> 「新增公钥」。

SSH 公钥默认存放在 `C:\Users\用户名\.ssh` 目录下的 `id_rsa.pub` 文件里，用记事本打开并复制其内容即可。

![CODING 新增SSH密钥](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/Coding%E6%96%B0%E5%A2%9ESSH%E5%AF%86%E9%92%A5.png)

新增密钥是敏感操作，所以在新增密钥的时候，CODING 会要求验证密码才能添加，需要注意的是这里要求验证的是**服务密码**，而不是登录密码。如果没有事先设置服务密码的话，需要先在「个人设置」–> 「服务密码」进行设置。

![CODING 设置服务密码](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CODING%E6%9C%8D%E5%8A%A1%E5%AF%86%E7%A0%81.png)

添加公钥后，打开 `Git Bash`，使用以下命令验证是否配置成功：

```bash
ssh -T git@e.coding.net
```

因为是第一次进行连接，所以需要输入 `yes` 来确认进行连接。配置成功，会出现如下图所示的提示：  
![CODING SSH 验证成功提示](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CodingSSH%E9%AA%8C%E8%AF%81%E6%88%90%E5%8A%9F.png)

## 部署 CODING「静态网站」

### 配置站点配置文件 \_config.yml

进入到 CODING 上刚才新建的项目里面，选择「代码仓库」，可以看到此时代码仓库还是空的，选择「SSH」链接方式并复制链接。

![CODING 项目 SSH 链接](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CODING%E9%A1%B9%E7%9B%AESSH%E9%93%BE%E6%8E%A5.png)

打开博客根目录下的站点配置文件 `_config.yml` ，修改其中的 `deploy` 区域，将刚复制的 SSH 链接新增进去：

```diff
deploy:
  - type: git
    repo: git@github.com:ousugo/ousugo.github.io.git
    branch: master
+ - type: git
+   repo: git@e.coding.net:asuradazone/asurada/asurada.git
+   branch: master    
```

### 部署代码到 CODING

`_config.yml` 文件修改完成后，执行 `hexo d` 命令进行部署，此时 Hexo 就会根据设置同时将博客部署到 GitHub 和 CODING 上面。

![成功部署到 CODING](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E6%88%90%E5%8A%9F%E9%83%A8%E7%BD%B2%E5%88%B0CODING.png)

此时再进入 CODING 上的项目，能看到刚才还是空的仓库现在已经有源代码了，说明部署是成功的。

![CODING 项目源码](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CODING%E9%A1%B9%E7%9B%AE%E6%BA%90%E7%A0%81.png)

### 项目启用「持续部署」功能

CODING 改版后，开启 Pages 服务的方法有了较大的变化，连名字都改了，现在已经不叫「Pages 服务」，而是改叫「静态网站」了。

![Pages 服务改名](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/Pages%E6%94%B9%E5%90%8D.png)

「静态网站」是「持续部署」下面的一个子功能，而「持续部署」功能默认是没有开启的，需要我们自行启用。启用路径为项目左下角「项目设置」–>「项目与成员」–>「功能开关」–>「持续部署」。

![启用持续部署功能](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E5%90%AF%E7%94%A8%E6%8C%81%E7%BB%AD%E9%83%A8%E7%BD%B2%E5%8A%9F%E8%83%BD.png)

「持续部署」功能开启后，就可以在它下面找到「静态网站」了。为了后续方便操作，还可以点击「静态网站」字样右边的按钮将其固定到项目选项顶部。

![静态网站入口](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E9%9D%99%E6%80%81%E7%BD%91%E7%AB%99%E5%85%A5%E5%8F%A3.png)

### 实名认证

CODING 现在要求是必须先进行实名认证以后才能使用「静态网站」的服务，实名认证的入口在「右上角头像」–>「团队管理」–>「团队设置」–>「高级设置」里面，藏得有点深。

如果**没有绑定手机号**的话，需要先绑定手机号才能进行实名认证，反正现在基本想要使用国内的服务，实名认证基本是跑不掉的。输入姓名和身份证号码即可完成认证。

![实名认证](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E5%AE%9E%E5%90%8D%E8%AE%A4%E8%AF%81.png)

### 发布静态网站

实名认证完成后，点击「立即发布静态网站」，填写「网站名称」，「触发机制」默认为「自动触发」，此后每次同步代码到 `master` 分支时，网站即会自动部署。所有设置保持默认即可，然后点击「保存」。

![静态网站设置](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E9%9D%99%E6%80%81%E7%BD%91%E7%AB%99%E8%AE%BE%E7%BD%AE.png)

设置保存后，刷新页面，会看到 CODING 已经自动生成了网站的访问链接，不过现在网站还没有正式部署，还需要我们点击「立即部署」来部署网站。部署完成后就可以点击链接查看网站部署效果了，应该同部署到 GitHub Pages 是同样的效果。

![部署静态网站](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E9%83%A8%E7%BD%B2%E9%9D%99%E6%80%81%E7%BD%91%E7%AB%99.png)

## 绑定自定义域名

我的自定义域名是在 [NmaeSilo](https://www.namesilo.com/) 买的，主要优点有：

1.  支持支付宝付款
2.  赠送 WHOIS 域名隐私保护
3.  第一年价格相对实惠

如果有同学也考虑在 [NmaeSilo](https://www.namesilo.com/) 购买域名，欢迎使用我的优惠码 `Asurada` ，可以优惠一美元。

### 修改域名解析服务到 DNSPod

之前博客只部署在 GitHub Pages 上时，因为不需要考虑国内外解析分流，所以我直接使用的是 NameSilo 默认的域名解析服务。现在博客分别部署在 GitHub 和 CODING 上了，我希望将国外的流量解析到 GitHub，国内的流量则解析到 CODING，但 NameSilo 默认的域名解析服务并不能实现国内外流量的区分，所以需要更换域名解析服务商。

我日常使用腾讯云比较多，所以选择了腾讯旗下的 [DNSPod](https://asurada.zone/post/Deploy-Hexoblog-To-Codingnet-And-GitHub/dnspod.cn)，账号和腾讯云是通用的，直接微信扫码即可登录。登录到 [DNSPod 控制台](https://asurada.zone/post/Deploy-Hexoblog-To-Codingnet-And-GitHub/console.dnspod.cn)，在「DNS 解析」–>「我的域名」里添加想要解析的域名。因为还没有修改域名注册商那边的设置，所以现在显示的状态还是「DNS 错误」。

![DNSPod 添加域名](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNSPod%E6%B7%BB%E5%8A%A0%E5%9F%9F%E5%90%8D.png)

登录到 [NameSilo 后台](https://www.namesilo.com/account_home.php)，在「Account Options」中选择「[domain manager](https://www.namesilo.com/account_domains.php)」，选中想要进行修改的域名，点击上方「Change Nameservers」图标。

![NameSilo 修改域名解析服务](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/NameSilo%E4%BF%AE%E6%94%B9%E5%9F%9F%E5%90%8D%E8%A7%A3%E6%9E%90%E6%9C%8D%E5%8A%A1.png)

将默认的 DNS 服务器地址修改为下面的 DNSPod 提供的 DNS 服务器地址，然后提交修改。

```dos
f1g1ns1.dnspod.net
```

```dos
f1g1ns2.dnspod.net
```

![修改域名服务商为 DNSPod](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E4%BF%AE%E6%94%B9%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%95%86%E4%B8%BADNSPod.png)

DNS 域名解析服务的修改后需要等待全球递归 DNS 服务器的刷新，所以需要耐心等待一会儿，大概十分钟左右。修改生效后再手动刷新 DNSPod 的状态，**注意不是刷新页面**，而是点击「DNS 错误」旁边的感叹号获取新状态，此时应该状态会变为「正常」。状态正常后，点击域名，进入「记录管理」。

![DNSPod 显示状态正常](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNSPod%E7%8A%B6%E6%80%81%E6%AD%A3%E5%B8%B8.png)

### DNSPod 解析记录设置

先简单介绍一下 `A 记录` 和 `CNAME 记录` 的作用：

> 如果需要将域名指向一个 IP 地址，就需要添加A记录；
> 
> 如果需要将域名指向另一个域名，再由另一个域名提供 IP 地址，就需要添加 CNAME 记录。

我们一共添加五条解析记录，其中三条境外，两条境内，具体记录如下：

![DNSPod解析记录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNSPod%E8%A7%A3%E6%9E%90%E8%AE%B0%E5%BD%95.png)

第一条记录类型为 `A`，主机记录为 `@`，解析线路选择`默认`，记录值为 CODING 静态网站的 IP 地址，可以使用 `ping` 命令查询得到。比如 CODING 替我自动生成的网站访问地址为 `http://quj3xn.coding-pages.com/`，打开 `cmd`，输入 `ping quj3xn.coding-pages.com`，就可以得到 CODING 静态网站的 IP 地址，我的是`124.156.205.241`， `TTL` 的话，免费版最小只能填 600。这条解析记录的意思是将从境内访问 `asurada.zone` 的流量解析到 `124.156.205.241`。

![获取 IP 地址](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E8%8E%B7%E5%8F%96IP%E5%9C%B0%E5%9D%80.png)

第二条记录类型为 `CNAME`，主机记录为 `www`，解析线路选择`默认`，记录值为 CODING 自动生成的访问地址，比如我的是 `quj3xn.coding-pages.com`。这条解析记录的意思是将从境内访问 `www.asurada.zone` 的流量解析到 `quj3xn.coding-pages.com`。

接下来的三条境外解析记录就很好理解了，第三和第五条记录是分别将从境外访问 `asurada.zone` 的流量解析到 `185.199.108.153` 和 `185.199.109.153`，这两个 IP 都是 GitHub Pages 的服务器 IP 地址。第四条记录则是将从境外访问 `ww.asurada.zone` 的流量解析到 `ousugo.github.io`。

在这一步的时候，为了方便下面开启 CODING 静态网站绑定自定义域名和开启全局 HTTPS，可以先将**境外的三条解析先暂停**，等完成 CODING 自定义域名的绑定并开启 HTTPS 以后再恢复解析。

### DNSPod「搜索引擎优化」

在「域名设置」–> 「功能设置」里有一个「搜索引擎优化」功能，默认是关闭的，说是和各大搜索引擎有深度合作，开启后可以优化站点收录，具体原理咱也不知道。但这是一个免费功能，所以不开白不开。

![DNSPod 搜索引擎优化](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNSPod%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E4%BC%98%E5%8C%96.png)

根据官方文档，主流的搜索引擎都在优化范围之内，看上去还是很不错的：

![DNSPod 搜索引擎优化名单](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNSPod%E6%90%9C%E7%B4%A2%E5%BC%95%E6%93%8E%E4%BC%98%E5%8C%96%E5%90%8D%E5%8D%95.png)

### CODING 绑定自定义域名并开启 HTTPS

进入 CODING 项目的静态网站设置页面，向下滚动找到「自定义域名」的设置项，绑定自定义域名。如果在绑定域名时出现证书申请错误：

> 申请错误原因是：在验证域名所有权时会定位到 GitHub Pages 的主机上导致 SSL 证书申请失败
> 
> 正确的做法是：先去域名 DNS 把 GitHub 的解析暂停掉，然后再重新申请 SSL 证书，大约十秒左右就能申请成功，然后开启强制 HTTPS 访问
> 
> — [绑定域名并开启 HPPTS](https://www.itrhx.com/2019/09/16/A47-hexo-deployed-to-github-and-coding/#5-%E7%BB%91%E5%AE%9A%E5%9F%9F%E5%90%8D%E5%B9%B6%E5%BC%80%E5%90%AF-HPPTS)

建议同时绑定有 `www` 前缀和没有 `www` 前缀的域名，并根据自己的需求将其中一个设为首选域名，另一个**勾选「跳转至首选」选项**，这样可以避免一些第三方服务数据统计不到一起。

如果两个域名的证书都没有问题的话，点击「强制 HTTPS」按钮即可启用强制 HTTPS 访问。设置完成后，不要忘记去 DNSPod 将之前暂停的 GitHub 解析重新开启。

![CODING 绑定自定义域名](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/CODING%E7%BB%91%E5%AE%9A%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9F%9F%E5%90%8D.png)

## 成果检验

至此，我们完成了博客在 GitHub Pages 和 CODING 静态网站服务上的双线部署。

首先进行 DNS 的查询，可以看见结果正如我们所期望的，从国内访问的时候走的是 CODING 的静态网站服务，而从境外访问的时候走的则是 GitHub Pages。

![DNS 解析结果](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/DNS%E8%A7%A3%E6%9E%90%E7%BB%93%E6%9E%9C.png)

最后自然是喜闻乐见的速度对比，这里使用的是[站长之家的国内网站测速工具](https://tool.chinaz.com/speedtest.aspx)。这里我们测试的域名分别是 CODING 和 GitHub **自动生成的网站域名**，多次测试下来，结果稍稍有些出乎我的意料，CODING 并没有如我想象的那样取得全面的、压倒性的速度优势，在部分地区反倒是 GitHub Pages 的访问速度更快。不过在多次测试中，CODING 的**平均响应时间**都比 GitHub 要快上三百到五百毫秒不等。

![速度对比](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E9%80%9F%E5%BA%A6%E5%AF%B9%E6%AF%94.png)

但如果是直接测试自定义域名 `asurada.zone` 的**国内访问速度**的话，分别设置域名**仅解析**到 CODING 和 GitHub，这时候可以看到解析到 CODING 时的**国内访问速度**明显比解析到 GitHub 时要快上不少。

我猜测出现这种情况可能是 DNSPod 解析的功劳——当直接使用 `quj3xn.coding-pages.com` 域名进行测速时，DNS 解析的速度比较慢，进而拖累了整体的访问速度，而当使用自定义域名 `asurada.zone` 进行测速时，此时由 DNSPod 负责进行域名解析，解析速度比较快，进而加快了整体的访问速度。

![解析到 CODING 时的网站访问速度](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E7%BD%91%E7%AB%99%E8%AE%BF%E9%97%AE%E9%80%9F%E5%BA%A6%E8%A7%A3%E6%9E%90%E5%88%B0CODING.png)

![解析到 GitHub 时的网站访问速度](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/%E7%BD%91%E7%AB%99%E8%AE%BF%E9%97%AE%E9%80%9F%E5%BA%A6%E8%A7%A3%E6%9E%90%E5%88%B0GITHUB.png)

我们更在意的自然是使用自定义域名访问时的速度，可以看到双线部署对于自定义域名的国内访问速度的提升效果是非常明显的，这次折腾也算是圆满成功，完结撒花✿✿ヽ(ﾟ▽ﾟ)ノ✿。

## 参考链接

[CODING 静态网站服务官方文档](https://help.coding.net/docs/devops/cd/static-website.html)

[Hexo 博客部署到 Coding](https://tding.top/archives/e09249d2.html)

[Hexo 双线部署到 Coding Pages 和 GitHub Pages 并实现全站 HTTPS](https://www.itrhx.com/2019/09/16/A47-hexo-deployed-to-github-and-coding/)
