---
title: Hexo博客备份方案
tags:
  - Git
  - Hexo
  - 博客备份
categories: 生命不息，折腾不止
cover: 'https://z3.ax1x.com/2021/04/12/crSYNQ.jpg'
abbrlink: f2411cb2
date: 2022-01-26 15:08:55
---
hexo是较为成熟的静态博客解决方案，但由于你所看到的网页中并不包含博客的源文件，这样对本地设备的依赖性很高，如果更换了设备想要维护在远端的博客就要在新设备上重新搭建，费时又费力。这个时候我们可以利用Git的分支系统来进行备份，这样一旦更换了新设备，只需要简单配置一下环境，然后git clone，就可以同步博客的源文件了。

首先我们要知道`hexo d`命令上传部署到GitHub上的文件是hexo编译过的，是用来展示网页内容的，所以并不包含本地的那些源文件。所以我们可以利用git将本地源文件上传到博客的另一个分支上进行备份。

- **在老设备上**
	1. 创建新的分支
		- 登录GitHub网站，在博客的仓库下新建一个分支，命名为backup
		- 在创建好分支后，在setting中将新建的分支设置为default
		> 不用担心会影响到`hexo d`push的分支，因为部署插件默认会部署到master分支上
		
	2. 配置用来备份的文件夹
		- 在本地的任意目录下执行`git clone https://github.com/username/uesrname.github.io.git`，由于默认分支已经设置为backup，所以只会克隆backup分支下的文件
		- 将克隆下来的目录中除了.git文件夹外的所有文件都删掉，如果看不到.git文件夹请打开显示隐藏文件夹
		- 将本地博客文件夹下除了.deploy_git的其他源文件全部复制过来
		> 如果之前克隆过themes中的主体文件，要将主题文件中的.git目录删除掉，否则无法备份主题文件
		
		- 在用来备份的目录下执行以下语句：
			```Bash
			git add .
			git commit -m "此次提交的备注"
			git push
			```
			
		这样你的hexo博客的目录就备份好了
		- **在新设备上**
			1. 安装Git并配置好全局变量
			2. 安装node.js，npm以及hexo，这时安装hexo就不需要初始化了
			3. 在任意目录下执行`git clone https://github.com/username/username.github.io.git`
			4. 进入到克隆好的文件夹执行：
				```Bash
				npm install
				npm install hexo-deployer-git --save
				```
				
			这样一来我们就可以利用git在多设备上来进行无缝的维护和编写博客了，也不用担心辛辛苦苦撰写的文章丢失了～
