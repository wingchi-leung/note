

### linux命令

#### cp

cp [options] source  dest

options: -r 递归复制目录

eg： cp -r  test  /root/wingchi/home

#### scp

在linux和win复制文件https://blog.csdn.net/denglavender/article/details/109631478





### git&命令行入门

> Git 是一个版本控制系统，是任何软件开发项目中的主要内容。通常有两个主要用途：代码备份和代码版本控制。你可以逐步处理代码，在需要回滚到备份副本的过程中保存每一步的进度. 

[git下载地址]https://git-scm.com/downloads  

`cd`： chang directory 转到目录 

`ls `: list list 

`pwd`：用于显示当前目录 



### git创建仓库

> 当你已经有了一个工程，并且预见了以后会不断有人修改和维护里面的源代码，这个时候你就需要用git来托管这个工程了。

建库可以先理解为创建一个目录，这个目录所有的文件都可以用git管理起来，并且文件的修改，删除都可以被追踪和还原。 

步骤：

1. 在本机创建一个空目录：

2. git创建版本库 : `git init ` 把一个目录变成git可以管理的仓库

   ![image-20201128155749609](D:\Typora\自服务\git.assets\image-20201128155749609.png)

3. 把文件添加到版本库：`git add readme.txt` 

用git commit告诉git，把文件提交到仓库上：`git commit  -m"A readme file" `  

 	`-m`： 本次提交的说明信息,不是必要但非常重要，是为了以后别人或者自己做版本维护容易理解 

​	注：在上传文件前要保证这个文件已经存在在当前目录，git不会帮你创建。 

#### git status 

查看状态

#### git diff

git diff `fileName`

#### git log 

可以查看提交历史，以确定要回退到那个版本

git commit id 是由SHA1计算出来的一个很大的数字，由16进制表示。  

#### git reflog

可以查看命令历史。以便确定回到未来的那个版本。 

### 文件上传 

我们已经上传了一个`readme.txt`文件，现在如果我们在文件内增加一句“learn git for future”并保存

回到git中可以通过`git status` 命令查看仓库当前状态 

![image-20201128161034926](D:\Typora\自服务\git.assets\image-20201128161034926.png)

可以看到文件已经被修改但没提交 



继续：使用 `git diff fileName.xxx`这个指令查看具体的改动 

![image-20201128161155487](D:\Typora\自服务\git.assets\image-20201128161155487.png)



接下来又可以上传文件了 

 ==提交修改和提交新文件是不一样的两步！== 

`git add readme.txt` 

`git commit -m"add something"` （上传一次快照）

然后再使用`git status` 来查看工作区，此时应该是干净的。 

#### git add 

`git add `指令是将当前目录下修改的所有的代码从工作区添加到暂存区

#### git commit 

而`git commit -m "..."` 是将暂存区的内容添加到本地仓库。  

每次使用git commit 指令我们都会在本地版本库生成一个40位的哈希值`commit-id`，这个值在版本回退时非常有用，它相当于一个快照,可以在未来的任何时候通过与git reset的组合命令回到这里. 



### 工作区

就是能在电脑中看到的目录

### 版本库

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

### 暂存区

> git的版本库中存了很多东西，最重要的是stage(或者叫index)的暂存区，和git为我们自动创建的第一个分支：master以及指向master的一个指针`head` 

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

把文件往git版本库里添加时，有两步：

1. `git add `添加文件，实际上把文件修改添加到暂存区
2. `git commit`提交更改，实际上是暂存区的**所有**更改内容提交到当前分支

 ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0) 



 ### git checkout撤销修改

git checkout -- `fileName` 意思是把fileName文件在工作区的修改全部撤销，这里有两种情况：

- 一种是readme.tet 修改后还没有被放到暂存区（git add），现在撤销修改就回退到上一次git add的状态。
- 一种是readme.txt已经添加到了暂存区（git add），但你又不想要这个修改了，这个时候需要先：
  1. git reset HEAD `fileName` 回退版本
  2. git checkout -- fileName 撤销修改。  

总之就是让文件回到最近一次git commit 或 git add时的状态。 

git checkout 其实是用版本库的版本替换工具区的版本。 所以如果不小心删掉了工作区的某个文件，可以用checkout恢复。







### 远程仓库

 ![git](https://www.ruanyifeng.com/blogimg/asset/2014/bg2014061202.jpg) 

当你在本地已经创建了一个Git仓库后，又想在github创建一个仓库，让这两个仓库进行远程同步。 

`git remote add origin git@github.com:usename/learngit.git`  



其中username/learngit替换成具体的用户名和仓库名

把本仓库的所有内容推送到远程库上：

`git push -u origin master` 

>  现在的github把默认的分支设置成了main，如果还是按照上面两个步骤操作，就会得到另外一个分支master，而不是直接推送到默认分支。 （会显示  master had recent pushed xxminutes ago)

> 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，可以在推送的同时把master分支设置为本地仓库当前分支的upstream。即会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

#### 删除远程仓库

`  git remote rm 远程仓库地址`

`  git remote rm origin`

#### 查看本地仓库的所有远程目录

`git remote -v` 

github上合并分支：

>  github仓库主页，有个`branches`点进去,分支后面有个`New pull request`，想合哪个分支点哪个。然后有个`create pull requesr`(填写完相关备注)，然后再`merge` 

#### git push

git push <远程主机名> <本地分支名>：<远程分支名>

如果远程和本地的分支名相同，可以省略冒号，即

git push <远程主机名> <本地分支名> <远程分支名>



`git push -u origin master`  将本地的master分支推送到origin主机的master分支



`$ git clone git@github.com:username/projectname.git`

#### git pull

用于从远程获取代码合并到本地的版本

git pull <远程主机名> <远程分支名>:<本地分支名>



### git upstream

场景：需要从其他仓库合并某个分支到现有仓库中。

只要把想要同步的远程仓库设置为本地仓库的upstream，就可以方便同步了。

什么是upstream：

 ![img](http://jalan.space/img/in-post/git-upstream.png) 

当你从github上clone一个别人的repo到本地，因为你不是repo的成员，所以无法向repo推送代码，此时对于本地仓库来说这个repo就是upstream 

当你把repo fork后，再clone到本地，此时你fork到自己的仓库的repo就是本地仓库的origin 

When a repo is cloned,it has a default remote called origin that points to your fork on github,not the original repo it was forked form 



To keep track of the original repo,you need to add another remote named upstream 



添加upstram

```git 
$ git remote add upstream https://github.com/octocat/Spoon-Knife.git
```

https://learnku.com/articles/48466

### 分支管理



创建分支：` git branch branchName `

切换到分支上： `git checkout dev` 

创建并切换到： `git checkout -b dev` 

查看当前分支： `git branch ` 会命令列出所有的分支，当前分支前面会标一个*号

git branch -r选项可以看远程的分支

git branch -a选项可以看所有的分支

合并分支： `git merge xx`   命令 ，把（指定）xx分支的工作合并到当前分支上 

删除分支：`git branch -d  branchName  `

 切换分支： 用switch比较科学

`git switch <branchName>`  



合并冲突：

可以通过git status 来查看冲突的文件

### 标签管理

发布一个版本时，我们通常在版本库打一个标签，确定版本。 

打标签: `tag V1.0` 

默认标签是打在最新提交的commit上的，有时候如果忘了打标签，可以找到历史提交的commit id 再打上。

`$ git log --pretty=oneline --abbrev-commit` 可以找到对应的id号

```
37e3a76 (HEAD -> master, tag: V1.0) conflict fixed
84a1ef0 add a line
1493eff (featurel) branch test
4bcc50e branch test
0aebc11 (origin/master, origin/HEAD) Initial commit
```



`git tag v0.9  84a1ef0` 

`git tag -a<tagName> -m"blablabla.."  -a指定标签名，-m指定说明文字`

删除标签： `git tag -d <tagName>` 







### 其他命令

` git log` 命令显示从最近到最远的提交日志  

`git log  --pretty=oneline `

`git reflog`查看命令历史，以便确定要回到未来那个版本。	



版本回退

`git reset --hard HEAD^` 

`git reset --hard 1094a` (1094a也就是每次修改的标识符（哈希值-id）写前几位即可，git会自动去找) 



>  Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`： 



  ![img](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg) 

### 在vscode配置git



### git配置记录

![image-20210107172023892](D:\Typora\自服务\git.assets\image-20210107172023892.png)

### 踩坑记录

https://stackoverflow.com/questions/2936652/git-push-wont-do-anything-everything-up-to-date



### 将本地的某个项目push到git上

在github上创建一个新仓库，不要加ReadMe！然后按照提示来做。

![image-20220508120031026](D:\Typora\自服务\git.assets\image-20220508120031026.png)