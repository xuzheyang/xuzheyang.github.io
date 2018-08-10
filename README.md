# 目录  

> * [I、申请GitHub帐号](#one)
> * [II、安装Git](#two)
> * [III、关联Git与GitHub](#three)
> * [IV、创建Pages项目](#four)

　　首先让我感谢网络上所有的文档、博客以及教程，让我参照着把自己的个人博客给搭建起来了，再此记下自己参照这些文档搭建自己博客的笔记。　　

<a name="one"></a>

# I、申请GitHub帐号  

　　既然是搭建`github pages`那么必须有一个`github`帐号是吧，没有的小伙伴[点击这里](https://github.com "github官网")官网申请，帐号把该填的填了就能申请的到。过程就不去描述了，这并不是今天的重点。  

<a name="two"></a>

# II、安装Git  

　　`gitbub`依赖`git`这个版本管理器，所以我们先安装版本管理器`git`。官网安装[点击这里](https://www.git-scm.com/downloads "git官网")，安装过程就不会请百度。  
　　安装完了之后不要忘了配置`git`，说是说配置，其实也就是告诉`git`当你提交修改的是后是谁在操作。  

> // 此处的账户跟`github`无关，但是建议用`github`的账号和用户名  
> $ git config --global user.name 用户名  
> $ git config --global user.email 邮箱地址  


<a name="three"></a>

# III、关联Git与Github  

　　用`Windows`的小伙伴打开`git-bash`，用`Linux`的小伙伴打开`bash shell`。先来看看有没有一个文件夹叫`.ssh`，如果有删掉。  

> $ ls -A | grep .ssh　　　// 查看有没有  
> rm .ssh -rf　　　　　　　　// 有就删掉没有跳过  

　　删掉之后需要重新建立一个ssh的钥匙，执行命令：  

> $ ssh-keygen  

　　输入之后的所有项全部可以直接回车用系统默认值。成功你会看到一个这样的图：  

![sshKey](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/ssh_keygen.PNG)  

　　现在用户主目录中会多一个隐藏的文件夹叫`.ssh`，进入这个文件夹可以看到一个`id_rsa.pub`的文件，打开它把内容复制。命令如下：  

> $ cd ~/.ssh  
> $ vi id_rsa.pub  

　　接下来打开进入`github`官网，
[点击此处](https://www.github.com "github官网")
在官网的右上角点击一个长的奇怪的按钮，后点击`settings`:  

![userSettings](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/usr_settings.PNG)

　　页面刷新过后，在页面左边有一个`SSH and GPG keys`点击他，右边页面刷新后可以在右上角看到一个`New SSH key`的按钮  

![newSshKey](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/new_ssh_key.PNG)  

　　`title`要不要无所谓，下面的`key`就把我们刚刚复制的`id_rsa.pub`文件里的内容粘贴上去就行了。点击`Add SSH Key`    

![addSshKey](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/add_ssh_key.PNG)

　　然后回到`git-bash`或者`bash shell`输入命令验证是否成功：  

> $ ssh -T git@github.com  

　　输入之后会出来一堆乱七八糟的东西，不要慌，直接输入`yes`就好了，之后就能看到`github`打招呼的内容了：  

![ssh_t](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/ssh_t.PNG)  

　　这就连接成功了，接下来我们要去`github`上新建项目了。  


<a name="four"></a>

# IV、创建Pages项目  

　　回到首页，右上角“+”按钮，点击他。选择`New repository`选项。  
　　项目名字的格式必须是`usrname.github.io`，只能是这样，如果你没有创建过这种项目的话，右边会是绿色的。顺便勾选下面的`Initialize this repository with README`，创建一个`README`文件再点击`Create repository`。  

![create_repo](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/create_repo.PNG)  

　　点击如图位置的`settings`  

![set_pro](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/set_pro.PNG)

　　刷新页面后，下拉。点击`Choose a theme`，在上面一行选择自己喜欢的主题点击右边的`Select theme`。

![theme](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/choose_theme.PNG)  

　　网页拉到最下面直接点击`Commit`

　　事实上现在你都可以浏览你的博客主页了，打开浏览器在地址栏输入`usrname.github.io`记得把`usrname`改成自己的用户名。是不是可以看到了呢？  
　　接下来我们继续，浏览器回到`github`中，打开创建的项目。右上角有一个`Clone or download`点击，若下面显示为`clone with SSH`就直接复制链接，不是的话点击右边的`Use SSH`后复制链接。  

![clone_ssh](https://github.com/xuzheyang/xuzheyang.github.io/raw/master/_pic/2017-01-09/clone_ssh.PNG)

 　　然后打开`git-bash`或者`bash shell`输入命令：  

> $ git clone xxx　　　 // xxx是刚刚复制的链接  

　　完成后，当前目录中会有一个与项目同名的文件夹，进入：  

> $ cd usrname.github.io  
> $ `vim _config.yml`

　　输入以下内容：  

> theme:jeky11-theme-merlot　　　//　原有不用改  
> baseurl:/xuzheyang.github.io　　//　自己添加，/项目的名字  

　　创建一个主页模板文件`index.html`:

> $ vim index.html

　　输入以下内容：  

~~~
<h2> {{ page.title }} </h2>
<p> 最新文章 </p>
<ul>
	{% for post in site.posts %}
	<li>
		{{ post.date | date_to_string }}
		<a href="{{ site.baseurl }}{{ post.url }}">
		</a>
	</li>
	{% endfor %}
</ul>
~~~

　　接下来创建一个文件夹`_posts`：  

> $ `mkdir _posts`  
> $ `cd _posts`  
> $ touch 2017-01-09-HelloWorld.md  

　　在这里可以用`Markdown`文件，也可以是`Html`文件，在文件中输入：  

~~~  
# test
这是一个用Marldown写的测试页  
~~~  

　　接下来就是添加上传了：  

> $ cd ~/usrname.github.io  
> $ git add *  
> $ git commit -m "Create Blogs"  
> $ git push  

　　现在就完成了，快去打开博客看看有没有`HelloWorld`的测试页面吧。
