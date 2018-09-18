
---
layout:     post   				    # 使用的布局（不需要改）
title:      git学习总结				# 标题 
subtitle:   study  #副标题
date:       2018-09-18 				# 时间
author:     BY wyy						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    
---



##1 查看远程分支 
git branch -a

	$ git branch -a
	* br-2.1.2.2
	  master
	  remotes/origin/HEAD -> origin/master
	  remotes/origin/br-2.1.2.1
	  remotes/origin/br-2.1.2.2
	  remotes/origin/br-2.1.3

##2 查看本地分支 
git branch

	shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)
	$ git branch
	* br-2.1.2.2
	  master

##3 创建分支 
git branch 分支名

	shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)
	$ git branch test
	 
	shuohailhl@SHUOHAILHL-PC /f/ggg/jingwei (br-2.1.2.2)
	$ git branch
	* br-2.1.2.2
	  master
	  test

##4.切换分支 
git checkout test

##5.版本回退 
git reset head  --版本号

##6.配置用户
git config --global user.name "xxx"
git config --global user.email "xxx"
git config --list

##7.常见错误
1.``fatal: Authentication failed for又不弹出用户名和密码 解决办法``
git config --system --unset credential.helper
可以重新填写用户名和密码进行提交了。来源：https://www.jianshu.com/p/8a7f257e07b8
