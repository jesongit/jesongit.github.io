---
title: 使用 Hexo + Github 搭建静态博客
date: 2020-03-22 15:24:47
categories:
  - 建站
tags:
  - hexo
---

> 提前安装好 GIT 和Node.js，其中 GIT 需要配置好 SSH key

#### 配置 hexo

**安装 hexo**

~~~cmd
npm install -g hexo-cli
~~~

**创建 blog 目录**

~~~cmd
hexo init blog
~~~

**安装相关依赖**

~~~cmd
cd blog
npm install
~~~

**基础配置**

~~~yml
# _config.yml
title: ''        # 标题
subtitle: ''     # 副标题
description: ''  # 描述
keywords: ''     # 关键字
author: ''       # 作者
language: zh-CN  # 语言
timezone: ''     # 时区
~~~

其他配置详见[官方文档](https://hexo.io/zh-cn/docs/configuration)

**本地启动**

~~~cmd
hexo s
~~~

在浏览器中输入 <http://localhost:4000> 即可查看欢迎页

#### 修改主题(可选)

> hexo 主题多种多样，只需要 clone 到 ./themes 中即可，操作步骤大同小异

这里以[diaspora](https://github.com/Fechin/hexo-theme-diaspora)主题为例

**安装主题**

~~~cmd
git clone https://github.com/Fechin/hexo-theme-diaspora.git themes/diaspora
~~~

**启用主题**

~~~yml
# _config.yml
...
theme: diaspora
...
~~~

详细配置可参考不同主题的文档

**预览主题**

~~~cmd
hexo clean
hexo g
hexo s
~~~

#### Github 绑定

**创建仓库**

创建一个名为<code>username.github.io</code>的仓库，其中 **username** 为你的用户名

![hexoblog-1](http://pic.jeson.club:39999/imgs/2020/03/a44afb59b9add87b.png)

**配置 Github 地址**

~~~yml
# _config.yml
deploy:
  type: git
  repo: git@github.com:jesongit/jesongit.github.io.git # 修改为自己的 ssh 链接
  branch: master
~~~

**部署到 Github**

~~~cmd
hexo g
hexo d
~~~

此时访问 <code>https://username.github.io</code> 已经可以浏览站点了

> 若出现 git 报错，ERROR Deployer not found: git，将 git 插件卸载再安装就好了
>
> ~~~cmd
> npm uninstall hexo-deployer-git --save
> npm install hexo-deployer-git --save
> ~~~

#### 域名绑定（可选）

> 有些同学可能对 <code>https://username.github.io</code> 这个域名并不满意，希望替换到自己域名

**域名申请**

>  推荐去国外的域名注册商除注册，不需要备案，非常快

演示已经在 [GoDaddy](https://sg.godaddy.com/zh) 注册好域名 <code>jeosn.club</code>

**配置CNAME**

在 <code>./source</code> 中创建名为 <code>CNAME</code> 的文件，保存申请的域名

~~~
www.jeson.club
~~~

上传 <code>CNAME</code>

~~~cmd
hexo d
~~~

**域名解析**

> 推荐使用[DNSPod](https://console.dnspod.cn/)（需要注册），GoGaddy提供的解析，<code>CNAME</code>无法添加 **@** 记录

在 **DNS 管理**中，修改自定义域名服务器为 <code>f1g1ns1.dnspod.net</code> 和 <code>f1g1ns2.dnspod.net</code>

![hexoblog-2](http://pic.jeson.club:39999/imgs/2020/03/2893603acfc40c65.png)

在 **DNSPod** 中添加 2 条 <code>CNAME</code> 记录即可

![hexoblog-3](http://pic.jeson.club:39999/imgs/2020/03/b10b899c3e5e37d4.png)

在浏览器中输入域名即可访问站点
> 图片支持基于 [ImgUrl](https://github.com/helloxz/imgurl) 自建图床