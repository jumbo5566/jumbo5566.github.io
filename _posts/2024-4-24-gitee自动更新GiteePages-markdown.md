---
layout: post
title: gitee自动更新GiteePages
subtitle: 非常有用
author: haha233jpg
categories: 教程
excerpt_image: "/assets/images/normol.gif"
tags: [教程]
top: 2
---

## 经历
 1. 方法介绍及代码修改
 2. 仓库设置
 3. 注意事项

## 1. 工具介绍
&emsp;&emsp;使用大佬仓库[ylb / gitee-pages-action](https://gitee.com/yanglbme/gitee-pages-action)完成自动更新Gitee Pages，网上教程大部分时间跨度太长，不适用于现在的环境，故此篇教程从0开始辅导小白完成该项目。

&emsp;&emsp;首先来到我们github的被gitee同步的仓库，在`.github/workflows`下创建一个文件，这里命名为`sync.yml`，接着可将大佬`Readme`下的代码部分复制到刚创建的`sync.yml`中保存。
![新建 图片](/assets/images/2024-4-24/同步代码.png)
&emsp;&emsp;如果是跟着我前两篇教程走到现在的朋友，可以精简掉大部分代码，仅保留这一部分，删除的一部分是自动同步github的代码到gitee，这部分功能已经完成，无需再进行配置。
~~~
name: Sync

on:
  push:
    branches: ["master"]
    workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: haha233jpg
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: haha233jpg/haha233jpg
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: master
~~~
&emsp;&emsp;`gitee-username`替换为自己的Gitee用户名,`gitee-repo`替换成自己的仓库。

## 2. 仓库设置
&emsp;&emsp;从我们的仓库页面来到`管理`，进入`Secrets and variables`并点击`New repository secrets`新建密钥
![gitee 图片](/assets/images/2024-4-24/仓库设置.png)
&emsp;&emsp;这里的`Name`填入`GITEE_PASSWORD`，`Secret`直接填入gitee账号的密码，然后点击`Add secret`完成设置。

## 3. 注意事项
&emsp;&emsp;**代码部分**这里需要注意，大佬的代码不能无脑复制粘贴，`branch`两处对应的参数`main`应修改为`master`。（需要来到我们的Pages界面查看`部署分支`，大部分人应该都是`master`，若为其他则需要相应替换）
![gitee 图片](/assets/images/2024-4-24/部署分支.png)

&emsp;&emsp;**密钥部分**：这一部分大佬没有介绍，但网上可以找到包含这一部分的教程，不过因为gitee版本问题，不完全一样，现版本`Secrets`变成了`Secrets and variables`，同时内容新增了一个`Environment secrets`，具有一定误导性，跟着我的教程操作即可。

&emsp;&emsp;**重要问题**：由于同步使用的是自带功能，与Page的同步方式不一致，如果先触发Page的同步，之后再触发仓库的同步会导致网页不会及时更新，一个方法是仓库同步也通过这种方法解决。这里我取了个巧，将触发方式修改为了`Push`，这样就不依赖github端刷新，而是gitee端push后刷新网页
~~~
name: Sync
on:
  push:
    branches: ["master"]
    workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: haha233jpg
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: haha233jpg/haha233jpg
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: master
~~~