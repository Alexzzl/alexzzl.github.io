---
title: 搭建 Hexo 博客
date: 2020-03-14 14:50:31
tags: 博客
categories: 博客
---
来源[知乎](https://zhuanlan.zhihu.com/p/35668237)
[原文](https://godweiyang.com/2018/04/13/hexo-blog/)
## 安装 Node.js
首先下载稳定版[Node.js](https://nodejs.org/dist/v9.11.1/node-v9.11.1-x64.msi)，我这里给的是64位的。

安装选项全部默认，一路点击`Next`。

最后安装好之后，按`Win+R`打开命令提示符，输入`node -v`和`npm -v`，如果出现版本号，那么就安装成功了。
## 添加国内镜像源
如果没有梯子的话，可以使用阿里的国内镜像进行加速。
```Bash
npm config set registry https://registry.npm.taobao.org
```
## 安装 Git

## 注册 Github

## 安装 Hexo
定位到博客文件目录下，输入`npm i hexo-cli -g`安装Hexo。会有几个报错，无视它就行。
安装完后输入`hexo -v`验证是否安装成功。
然后就要初始化我们的网站，输入`hexo init`初始化文件夹，接着输入npm install安装必备的组件。

这样本地的网站配置也弄好啦，输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器
## 连结 GitHub 与本地
首先右键打开git bash，然后输入下面命令：
```Bash
git config --global user.name "Alexzzl"
git config --global user.email "805119233@qq.com"
```
用户名和邮箱根据你注册github的信息自行修改。

然后生成密钥SSH key：
```Bash
ssh-keygen -t rsa -C "805119233@qq.com"
```
打开[Github](https://github.com/)，在头像下面点击settings，再点击SSH and GPG keys，新建一个SSH，名字随便。

git bash中输入

```Bash
cat ~/.ssh/id_rsa.pub
```
将输出的内容复制到框中，点击确定保存。

输入`ssh -T git@github.com`，出现你的用户名，那就成功了。

打开博客根目录下的_config.yml文件，这是博客的配置文件，在这里你可以修改与博客相关的各种信息。

修改最后一行的配置：
```Bash
deploy:
  type: git
  repository: https://github.com/alexzzl/alexzzl.github.io
  branch: master
```
repository修改为你自己的github项目地址。

## 写文章、发布文章
首先在博客根目录下右键打开git bash，安装一个扩展`npm i hexo-deployer-git`。

然后输入`hexo new post "article title"`，新建一篇文章。

然后打开`blog\source\_posts`的目录，可以发现下面多了一个文件夹和一个`.md`文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。

编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到github上。这时打开你的github.io主页就能看到发布的文章啦。

## 绑定域名

## 备份博客源文件

有时候我们想换一台电脑继续写博客，这时候就可以将博客目录下的所有源文件都上传到github上面。

首先在github博客仓库下新建一个分支`hexo`，然后`git clone`到本地，把`.git`文件夹拿出来，放在博客根目录下。

然后`git checkout hexo`切换到`hexo`分支，然后`git add .`，然后`git commit -m "xxx"`，最后`git push origin hexo`提交就行了。

 
## 个性化设置（matery主题）