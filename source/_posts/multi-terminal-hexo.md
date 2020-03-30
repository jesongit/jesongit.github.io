---
title: 多终端编辑 hexo 博客
date: 2020-03-31 00:23:44
categories:
  - 建站
tags:
  - hexo
  - git
---

>   默认已构建好 `hexo` 博客，此处通过分支对源文件进行管理，也可以另建仓库管理

**配置 `.gitignore` 文件（可选）**

>   出于安全考虑，不上传配置文件

~~~
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
_config.yml
~~~

**重新进行版本控制**

~~~shell
# 注意替换为自己的仓库
git init                   # 初始化本地仓库
git add -A                 # 添加本地所有文件到仓库        
git commit -m "blog源文件"  # 添加 commit
git branch hexo            # 创建本地仓库分支 hexo
# origin是本地分支,remote add操作会将本地仓库映射到云端
git remote add origin git@github.com:jesongit/jesongit.github.io.git
git push origin hexo       # 将本地仓库的源文件推送到远程仓库 hexo 分支
~~~

**其他终端**

>   默认配置好 `SSH KEY`，安装好 `Node.js` 等环境

~~~shell
git clone -b hexo git@github.com:jesongit/jesongit.github.io.git
cd hexo
npm install
~~~

**上传源文件**

~~~shell
git add *
git commit -m "update"
git push
~~~

**同步源文件**

~~~shell
git pull origin hexo
~~~

