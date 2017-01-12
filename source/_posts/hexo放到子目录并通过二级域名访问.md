---
title: hexo使用、二级域名访问及多终端编辑
date: 2016-12-28 02:09:31
categories:
- hexo
tags: 
- hexo 
---
### hexo部署
先说一下，如果你GitHub主页原来是有项目的，那千万别直接`hexo d -g`到你的主页所在项目里。

**千万别！千万别！千万别！**
<!-- more -->
因为hexo部署是`git push --force`强制提交的，会覆盖掉你原来的项目，包括提交记录，如果你本地仓库也没有了，那是很绝望的。别问我怎么知道的（还好我本地仓库完好，又`git push --force`回去了。

### Next主题
无论是hexo还是我使用的主题Next的文档都很详尽，特别是Next的文档，所以虽然会踩一些坑，但是也能很快搭建起来。除了有些CSS样式错误，直接在Chrome调试工具定位CSS文件，都能自己改一下。所以直接看文档，有问题也可以留言给我

### 二级域名访问
我的GitHub主页已经有很多demo页面，我又不想在动这些demo，所以我用hexo的时候想在根项目下建立一个blog目录用于存放博客的静态页面。google了一下发现这样就只能手动`push`public目录到blog。后来发现可以新建一个项目并通过`blog.strongfanfan.top`这种二级域名访问，是不是比`www.strongfanfan.top/blog`逼格稍微高一点。下面我就来说说是怎么实现的。
#### 1. 个人主页和项目主页

个人主页只能有一个，而项目主页可以有很多个

你的每个项目Settings\>GitHub Pages\>Source默认选项是`none`，改为`master branch`，点击`save`就可以通过`yourName.github.com/repoName`访问。
#### 2. CNAME文件
   
   在你的项目根目录下创建CNAME文件，没有文件后缀。内容写上你的二级域名，不带协议，即`http://`，比如我的内容就是`blog.strongfanfan.top`，因为deploy部署可能会删除这个文件，所以最好放在hexo根目录下source文件夹下。
 #### 3. DNS解析
   
   最后就是给你的一级域名解析添加一行`CNAME`记录，主机记录为你想要的二级域名字段，比如我的是`blog`，记录值就是你的GitHub主页，比如`strongfanfan.github.io`，保存以后等几分钟就可以通过你的二级域名直接访问你的博客了

---------------------------------------------------
  
### Front-matter
最后贴一些日常写作可能用到的Front-matter，方便以后查找。
> #### md模板预定义参数

> |参数|描述|默认值|
>|:---:|:---:|:---:|
>|  layout|	布局||	
>|title|	标题||	
>|date|	建立日期|	文件建立日期
>|updated	|更新日期	|文件更新日期
>|comments|	开启文章的评论功能|	true
>|tags|	标签（不适用于分页）||	
>|categories|	分类（不适用于分页）||	
>|permalink|	覆盖文章网址| .|	

>  #### 分类和标签

 >只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 Foo, Bar 不等于 Bar, Foo；而标签没有顺序和层次。
```md
categories:
- Diary
tags:
- PS3
- Games
```

--------------------
更新于2017/1/12

### 多终端编辑

新建一个项目或者在blog项目下新建分支，用于存放博客源码，每次部署完博客网页，`push`一下源码到分支，在公司电脑或者新机器上`clone`下来，执行`npm install`命令就可以同步编辑部署了。
下次就直接`pull`一下远程仓库的版本就可以了。