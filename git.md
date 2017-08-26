
集中式：cvs svn
分布式：git

询问版本信息
 git version

指定整个电脑的git仓库的用户名和邮箱
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

创建版本库

    pwd 显示当前的位置

初始化一个git仓库
$ git init

添加文件到git仓库，分两步：
   step one： git add <file>  #可以反复多次使用，添加多个文件
   step two： git commit -m "你所要备注的信息"
   
时光机穿梭
$ git status #查看仓库当前的状态

$ git diff <file.md> #查看修改内容

版本回退
$ git log  #查看提交历史，显示从最近到最远的提交日志

$ git reset --hard commit_id #在版本的历史之间穿梭

$ git reflog #查看命令历史，以便确定要回到未来的哪个版本

工作区和暂存区

工作区(working directory)  
版本库(repository)

暂存区 #git add 的文件先到暂存区(stage)
      #git commit 之后进入（master)


	 
	 
	 
管理修改
git跟踪并管理的是修改，而非文件。

提交后 用 git diff HEAD -- <file>命令可以查看工作区和版本库里面最新版本的区别

如果不add到暂存区，那就不会加入到commit中



撤销修改

$ git checkout -- <file> 
#把<file>文件在工作区的修改全部撤销，两种情况：
# 一种是<file>文件还没有放入暂存区，撤销修改就回到和版本库一模一样的状态
#一种是<file>文件已经添加到暂存区，又作了修改，现在撤销修改就回到添加到暂存区后的状态
# 总之，就是让这个文件回到最近一次git add或git commit时的状态


$ git reset HEAD <file> #可以把暂存区的修改撤销掉(unstage),重新放回工作区

删除文件

$ rm 1.md #删除文件

 # 确实要删除版本库中的对应的文件
$ git rm 1.md 
$ git commit -m"remove 1.md"

 # 删错了
$ git checkout -- 1.md


git杀手级功能之一：远程仓库！！！

github 注册一个github账号，就可以免费获得git远程仓库

 本地git仓库和github仓库之间的传输是通过ssh加密的，所以：
    step one：创建ssh key:
	ssh-keygen -t rsa -C "youremail@example.com"
	然后到用户主目录下的.ssh中，找到id_rsa和id_rsa.pub文件
    step two: 到github中添加ssh key

添加远程库
情景：你在本地创建了一个git仓库后，又想在github创建一个git仓库，
      并且让这两个仓库进行远程同步

要关联一个远程库，使用命令：
$ git remote add origin git@github.com:fromddy/learingit.git

关联后，使用如下的命令  第一次推送master分支的所有内容:
$ git push -u origin master

Counting objects: 19, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (19/19), done.
Writing objects: 100% (19/19), 13.73 KiB, done.
Total 23 (delta 6), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.

从现在起，只要本地做了提交，就可以通过命令：
$ git push origin master

从远程库克隆

$ git clone git@github.com:bluemainland/learngit.git


!!!分支管理

Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


解决冲突
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000


分支管理策略
$ git merge --no-ff -m"merge with no-ff" dev 

Bug分支
当手头的工作没有完成时，先把工作现场git stash一下，然后去修复bug
修复之后，用git stash pop，回到工作现场。

$ git stash list #查看有多少个工作现场

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

$ git stash apply stash@{0}

feature分支
$ git branch -D <name>  #强行删除某个未merge的分支

多人协作

查看远程库的信息，用git remote：
$ git remote 
   origin 
   
用git remote -v显示更详细的信息：
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。


推送分支

$ git push origin master #把本地的master 分支提交到远程库

$ git push origin dev  #把本地的dev分支提交到远程库


抓取分支

多人协作时，大家都会往master和dev分支上推送各自的修改。

现在，模拟你的一个小伙伴，可以在另一台电脑上(注意把ssh key添加到github)
或者同一台电脑的另一个目录下克隆：

$ git clone git@github.com:michaelliao/learngit.git

当你的小伙伴从远程库clone时，默认你的小伙伴只能看到本地的master分支。

$ git branch 
* master

现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

$ git checkout -b dev origin/dev


因此，多人协作的工作模式通常是这样：

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

http://strivingboy.github.io/blog/2014/09/29/better-git-log/


标签管理

创建标签
$ git branch 
*dev
 master
$ git checkout master

$ git tag v1.0 #创建一个新的标签v1.0

$ git tag #查看所有的标签

$ git tag v0.9 commit_id  #为commit_id对应的commit打标签

$ git log 00pretty=oneline --abbrev-commit

$ git show v0.9  #查看标签信息

$ git tag -a v0.1 -m "information" commit_id #创建带有说明的标签，用-a指定标签名，-m指定说明文字

   #用命令git show <tagname>可以看到说明文字
   
$ git tag -s v0.9 -m"signed version 0.2 released" commit_id
    #可以通过-s用私钥签名一个标签
	#签名采用PGP签名，首先必须安装gpg(GnuPG)

操作标签

$ git tag -d v1.0 #删除标签

$ git push origin v1.0 #推送某个标签到远程

$ git push origin --tags #一次性推送全部尚未推送到远程的本地标签

如果标签已经推送到远程：

$ git tag -d v9.0 #首先从本地删除

$ git push origin :refs/tags/v0.9 #从远程删除


自定义git
让git显示颜色，会让命令输出看起来更醒目：
$ git config --global color.ui true

忽略特殊文件 
.gitignore文件

$ git add -f App.class #即使在.gitignore文件中，强制添加到git

$ chek-ignore -v App.class #检查是不是.gitignore写得有问题

配置别名

$ git config --global alias.st status

搭建git服务器
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000
等我有一个服务器的时候再说吧

