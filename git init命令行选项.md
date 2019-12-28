### git init命令行选项



```
git init [-q | --quiet] [--bare] [--template=<template_directory>]
	  [--separate-git-dir <git dir>]
	  [--shared[=<permissions>]] [directory]
```

一旦执行了git init，它就会创造一个新的空的仓库`.git`，在这个仓库下有几个主要的子目录（`refs/heads`,`refs/tags`），且创造了指向master的HEAD头子针

如果在已有仓库里执行git init并不会覆盖已经存在的东西



1.`git init`：执行命令后，会在执行此命令的当前目录下创建一个`.git`文件夹，且在`.git`同级目录下用于存放源码（包含`.git`和源码的目录，通常称为工作目录，工作目录是一个包含有版本历史目录“.git”和源文件的目录。），这样可以进行版本管理（修改代码，更新代码等等）

![](/home/zy/Pictures/init0.png)

在`.git`文件夹下有这几个文件、文件夹

![](/home/zy/Pictures/init-1.png)

如果在原有仓库下，重新执行`git init`，会提示`Reinitialized existing Git repository in /home/${user}/.git/`，并不会清空之前的设置等内容。

2.`git init -q | --quiet`：仅仅打印错误信息，其他的信息不打印

3.`git init --bare  `：创建一个裸仓库，也就是直接把`.git`里的各个文件直接放在当前执行此命令的目录下（**此时没有`.git`这个文件夹了）**，仅仅记录版本历史，不含项目源文件，也即是没有工作区，用作分享版本库，通常在服务端上创建一个bare仓库

![](/home/zy/Pictures/init-1.png)

#### 远端仓库初始化成bare仓库

远端仓库的用户正在master的分支上操作，而你又要把更新提交到这个master分支上，当然就出错了。

但如果是往远端仓库中空闲的分支上提交还是可以的，比如

git push origin master:b1   还是可以成功的

解决办法就是使用”git init –bare”方法创建一个所谓的裸仓库，之所以叫裸仓库是因为这个仓库只保存git历史提交的版本信息，而不允许用户在上面进行各种git操作

这就是最好把远端仓库初始化成bare仓库的原因。



**下面将解释在本地模拟创建git服务器和git客户端的详细步骤**

###### 一、服务端准备

1.在本地创建一个裸仓库（只有版本库信息，没有项目源码文件），以我的为例子（`/home/{user}/demo`）

```
//进入demo文件夹下
$ cd demo

//创建demo.git文件夹
$ mkdir demo.git

//进入demo.git文件夹下
$ cd demo.git

//初始化裸的git，也就是没有代码的git仓库
$ git init --bare
//列出默认初始化裸的仓库所生成的文件
$ ls
branches  config  description  HEAD  hooks  index  info  objects  refs
```

2.创建hooks(钩子)文件`post-receive`

```
//查看当前路径
$ pwd
/home/{user}/demo/demo.git

//进入hooks文件夹下
$ cd hooks/

//创建post-receive文件，并输入你想要的脚本操作
$ cat > post-receive
!#/bin/sh
$ git --work-tree=/home/{user}/demo/code --git-dir=/home/{user}/demo/demo.git 
/* --work-tree：代表的是工作区路径(客户端提交文件后，文件存储的位置)；--git-dir：代表git仓库路径，也就是.git文件夹的路径（存储.git文件夹下的文件）*/

//上方文件创建后，按住ctrl+D即可保存（或者也可用vim来编辑）

//给钩子文件添加执行权限
$ chmod +x post-receive
```

###### 二、客户端准备

1.添加远程分支

```
//进入客户端
$ cd /home/{user}/git

//如果你是从github上克隆下来的，那么就可以直接做如下操作（因为已经存在.git）
$ git remote add demo /home/{user}/demo/demo.git  //add后的demo代表远程分支名；demo后的路径代表远程分支的https/ssh/本地的地址，其意思：创建远程分支名为demo，并将其与远程分支地址相关联


//如果你本地没有克隆git，想从无到有，那么你将这么做
//先初始化git
$ git init

//添加远程分支
$ git remote add demo /home/{user}/demo/demo.git

//如果远程分支有文件，那么应该先拉取远程分支文件，否则出错
To /home/{user}/demo/demo.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to '/home/{user}/demo/demo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first merge the remote changes (e.g.,
hint: 'git pull') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

所以在push前，需要先pull拉取远程文件，并且拉取远程文件还不能git pull demo（如果这样做，那么将报错），必须指定是哪个分支
$ git pull demo master


//然后就是提交文件到远程仓库了，如git push demo将报错：
warning: push.default is unset; its implicit value is changing in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the current behavior after the default changes, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (7/7), 684 bytes | 0 bytes/s, done.
Total 7 (delta 2), reused 0 (delta 0)
To /home/{user}/demo/demo.git
   1fb01c7..adffdb1  master -> master

//所以需要按正式来写，后面必须加上是分支中的哪个分支，否则将无法提交
$ git push demo master
```

2.提交文件到远程仓库

```
//将本地的master分支提交到远程仓库名为demo的master分支
$ git push demo master:master
也可以简写为git push demo master
```

到这里为止，本地git服务器（demo）和客户端( git )算是搭建好了，这时，你在本地创建文件，提交文件，提交到远程仓库这个过程已经可以玩透了（也即是，你在客户端修改文件，然后提交到服务器；服务器一查看，发现文件已经存在或者修改了）。但是我此时发现，在服务器修改的文件，客户端更新后却没有拉取到最新文件，于是做了如下操作

```
//进入服务器代码区
$ cd /home/{user}/demo/code

//初始化git（因为从客户端提交的文件没有包含.git）
$ git init 

此时，服务器端修改文件后，也需要和客户端一样，git add | commit | push demo master，将文件提交到服务器，让服务器记录历史

//进入客户端
$ cd /home/{user}/git

//拉取服务器数据，更新本地代码
$ git pull demo master

此时，查看客户端发现，代码已经发现了变化，也即是从服务器上拉取更新文件成功了！！！
```

至此，从客户端到服务器，从服务器到客户端的文件相互传输也就完成了～～～



**总结：其实服务器你可以理解为demo.git；服务器获取的客户端提交的文件可以理解为code（服务器修改后本客户端拉取更新，其实就类似客户端那样）；最后客户端就是git啦；所以这个过程最少就需要用到三个`.git`。**

demo.git就像是一个中介，他只负责从客户端的文件传输，然后将其文件放置在code里；然后code和git就代表两个客户端。这样子就构成了类似我们在github上提交文件一样，文件在服务器和客户端之间的相互实时传输。

同时也实现了git和代码之间的分离～～～



**创建的裸仓库，可供其他地方pull和push**



**git init 初始化服务端仓库无法即时检出更新的问题**https://blog.csdn.net/sinat_34349564/article/details/52486886



4.`git init --shared[=(false|true|umask|group|all|world|everybody|0xxx)]`

这个选项指定Git存储库将在多个用户之间共享。这允许属于同一组的用户推入存储库。（用于指定仓库是否要分享给多个用户共享）；没有指定时即`git init --shared`，默认值是umask



5.`git init --template=<template_directory>`：模板目录？？？(暂时不知道有什么作用)