---
layout: post
title: 如何使用Jekyll在github上搭建博客
tags: Jekyll，github，博客
date: 2018-07-23 17:33:58 +0800
categories: Jekyll
---


简单说，只需要三步，就可以在 Github 搭建起一个博客：

1. 在 Github 上建一个名为 xxx.github.io 的库；
1. 把看中了的 Jekyll 模板 clone 到本地；
1. 把这个模板 push 到自己的库；

下面为了从头展示如何用 Git + Github + Jekyll 搭建博客。

### 一、在 Github 创建名为 username.github.io 的库 

按照 Github Pages 上的说明，首先要创建一个新的库，把它命名为 username.github.io。博客搭建成功后，这就是该博客的访问网址。

库名的第一部分需要与用户名一致才能生效。所以如果你的用户名是 MichaelMaoMao，库的名字就是MichaelMaoMao.github.io。

![01](img/01.png)

关于「Initialize this repository with a README」这个选项是在初始化库的时候创建一个关于该库的说明，huangziwei建议不要勾选，自己提交一个，我为了省事勾选了，

如果你没有勾选，可以以后创建

第一步完成，创建好了库，但是里面空空的，没什么东西，下面就要用到git工具向仓库存放主页和其他文件啦。

如果对git不熟悉的童鞋，可以参考 git教程，也可以在终端中输入 git help 命令来查看所有的命令。　　

如果你之前没有勾选README，现在可以打开终端，

cd 到桌面，然后复制一下你库的地址：

![02](img/02.png)

  然后clone到桌面

    　　git clone https://github.com/username/username.github.io.git

  进入本地库

　　    cd username.github.io.git

 　输入以下命令创建一个README.md：

    　　echo "# username.github.io" >> README.md

    　　git add README.md

    　　git commit -m "first commit"

    　　git push origin master

 
### 二、选择模板

jekyllthemes.org 上有很多Jekyll模板，寻找自己喜欢的。我用的模板是 Skinny Bones，这部分huangziwei讲的很清楚，我就直接用他的解释吧（他创建的库名是hyaojia）： 

首先，我们把 Scribble 这个库 clone 到本地：

    $ git clone https://github.com/hyaojia/scribble.git

把名为 scribble 的文件夹改名为 hyaojia.github.io (不必要，理由跟之前一样，只是为了比较好找)，只需要下面这行命令（mv 是移动文件夹的命令，可是也能用来重命名文件。

我明白，一开始这很难理解）：

    $ mv scribble hyaojia.github.io
然后我们可以进入现在叫 hyaojia.github.io 的文件夹里:

    $ cd hyaojia.github.io

### 三、把博客托管到 Github Pages

一般而言，克隆了别人的模板，第一件事要做的就是修改 _config.yml 里的个人信息。在 scribble 这个模板中，修改的地方不多，只需要把导航栏相关的连接修改成自己的就可以了，

比如 Blog 的 url 改成 http://hyaojia.github.io，邮件和 Github 帐号改成自己的帐号。

只要修改过文件，我们就需要重复 git add 和 git commit 这两步：

    $ git add . 
    $ git commit -m 'modified _config.yml'

git add . 里的一点，指把当前目录所有修改过的文件都加到 Staged Area 去。

前面我们说过 git remote -v。因为我们直接 clone 了别人的库，所以 clone 下来的文件夹里，已经登记了模板作者的远程库信息：

    $ git remote -v
    origin	https://github.com/muan/scribble.git (fetch)
    origin	https://github.com/muan/scribble.git (push)

我们要把 origin 的地址改成我们之前创建的 username.github.io 库的地址：

    $ git remote set-url origin https://github.com/username/username.github.io.git
    $ git remote -v
    origin	https://github.com/username/username.github.io.git (fetch)
    origin	https://github.com/username/username.github.io.git (push)

现在已经变成我们自己的了。

最后输入

    $ git push -u origin master

如果没有绑定 SSH key，一般会要求输入用户名和密码。输入后则会出现

    Counting objects: 268, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (187/187), done.
    Writing objects: 100% (268/268), 224.02 KiB | 0 bytes/s, done.
    Total 268 (delta 76), reused 268 (delta 76)
    To https://github.com/hyaojia/hyaojia.github.io.git
    * [new branch]      master -> master

代表成功推送。现在在浏览器输入网址 http://username.github.io，则可以看到博客的样子了。

如果没有修改模板的需求，利用 Git + Github + Jekyll 搭建博客大概就是这样子。写文章，只需要在 _post/ 文件加中，加入带有 YAML 头信息（YAML front matter）的 markdown 文件，然后 push 到 Github，就会被自动渲染成 HTML。比如，增加一篇名为 My First Post 的博客，在本地创建一个文件名带有日期的 markdown 文件 2015-04-20-my-first-post.md（里面要写好头信息）：

    ---
    layout: post
    title: My First Post 
    ---
	
这是我的第一篇博客

最后按上述方法(git add / git commit / git push)推送到 Github，就大功告成了。

 

### 四、添加评论
 
一个博客少不了交流，如果你想让别人评论，可以选择将评论托管给Disqus。

- 访问Disqus注册帐号，并验证邮箱;

- 登录后点击Add Disqus To Site ,

![03](img/03.png)     

   填写所连接博客的名字，选择一个disqus的url (最好和你的用户名一致，方便查看):
　　
![04](img/04.png)
 
- 然后在你本地github.io文件里修改_config.yml配置文件，在disqus_shortname：后面添加你的disqus名称:

![05](img/05.png)　　

- 最后修改好之后，add, commit, push到github, 过一会刷新一下就可以看到下面的评论区了。enjoy,  ;)

附上我刚建好的小站, 欢迎留言： [http://michaelmaomao.github.io][http://michaelmaomao.github.io]



参考链接： 

[huangziwei的博客, 很耐心的教程][huangziwei的博客, 很耐心的教程]

[isnowfy做一个博客theme][isnowfy做一个博客theme]

[搭建Disqus评论区][搭建Disqus评论区]

[git教程][git教程]

[如何搭建一个独立博客][如何搭建一个独立博客]


[http://michaelmaomao.github.io]: http://michaelmaomao.github.io
[huangziwei的博客, 很耐心的教程]: http://huangziwei.com/tech/blogging-with-git-github-and-jekyll/
[isnowfy做一个博客theme]: http://isnowfy.github.io/about-simple-cn.html
[搭建Disqus评论区]: http://terryleeorz.github.io/how-to-build-blog.html
[git教程]: http://git-scm.com/book/zh/v2
[如何搭建一个独立博客]: http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/