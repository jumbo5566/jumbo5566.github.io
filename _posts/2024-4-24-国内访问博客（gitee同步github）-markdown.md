---
layout: post
title: 国内访问博客（gitee同步github）
subtitle: 非常有用
author: haha233jpg
categories: 教程
excerpt_image: "/assets/images/normol.gif"
tags: [教程]
top: 2
sidebar: []
---

## 教程经历
 1. gitee仓库创建
 2. gitee page使用
 3. 自动同步github仓库
 4. 获取Github仓库地址
 5. 注意事项

## 1. gitee仓库创建
&emsp;&emsp;主页右上角`+`选择`新建仓库`
![新建 图片](/assets/images/2024-4-24/新建.png)
&emsp;&emsp;二级页面中右上角`点击导入`
![新建仓库 图片](/assets/images/2024-4-24/新建仓库.png)
&emsp;&emsp;接着在`导入仓库`界面`Git仓库URL`栏填上上一章我们在Github中创建的仓库地址（获取方法在文章末尾处）
![导入仓库 图片](/assets/images/2024-4-24/导入仓库.png)
&emsp;&emsp;同时注意仓库名称必须与gitee用户名一致，不然最后生成的网页域名又臭又长，接着仓库可以设置成开源，完成后导入即可。（导入源是github，所以需要用点科技，后续操作如果卡住，也可试试科技）

## 2. gitee page使用
&emsp;&emsp;在我们的仓库页面可以看到`服务`选项，选择`Gitee Pages`
![gitee 图片](/assets/images/2024-4-24/giteepage.png)
&emsp;&emsp;第一次使用会提示实名，按流程操作等待即可
&emsp;&emsp;Gitee Pages页面只需要勾选`强制使用HTTPS`即可，确定后稍作等待
![启用 图片](/assets/images/2024-4-24/启用page.png)
&emsp;&emsp;这里可以看到出现我们网站的域名，接着便可以通过该域名正常访问我们的博客（无需科技）
![域名 图片](/assets/images/2024-4-24/出现域名.png)

## 3. 自动同步github仓库
&emsp;&emsp;在该仓库页面点击`管理`进入设置，之后点击仓库镜像管理，右上角`添加镜像`
![镜像 图片](/assets/images/2024-4-24/镜像.png)
&emsp;&emsp;镜像方向出选择`Pull`(从github将仓库同步到这里)，`镜像仓库选择上节中创建好的仓库`，最后`获取私人令牌`，此处有官方教程，不再赘述，最后点击`添加`完成仓库同步。

## 4. 获取Github仓库地址
&emsp;&emsp;来到我们的Github仓库界面，在`code`处这一串https地址即为我们的仓库地址
![github仓库地址 图片](/assets/images/2024-4-24/github仓库地址.png)

## 5. 注意事项
&emsp;&emsp;**实名部分**：实名要求很严格，身份证照片简单，但是手持证件照有一定难度，拍摄时必须保证身份证显示清晰（真的不能有一点模糊，我就是因为模糊，审核从星期三到了星期三，花了一个星期，因为周末不审核，一次2天起步），不想多花时间这一步要好好对待，人像可以模糊一点，同时必须露出手臂，上面有范例图片。
&emsp;&emsp;**获取令牌部分**：这里切记选择第二项（因为官方教程是按第二个来的），不然会找不到对应设置。
![令牌 图片](/assets/images/2024-4-24/令牌.png)
&emsp;&emsp;**文档编辑部分**：我们使用gitee同步仓库是为了国内直接访问，但是这个主题使用的图片和视频一部分来源是网络，国内无法访问，所以我们需要将对应资源下载下来放到仓库，这里推荐将图片和视频放到`/assets/images`和`/assets/videos`文件夹下。同时需要注意，在编辑markdown文件时，这部分地址需要使用`" "`包起来，不然主页的博客不会显示对应帖子的预览图。
![预览 图片](/assets/images/2024-4-24/预览图.png)