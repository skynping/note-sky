> <p style="color:blue">以下内容参考于[廖雪峰的博客](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)</p>

---

# <center style="color:blue">git教程</center>

**安装**
linux下安装：sudo apt-get install git
window下安装：从官网下载即可

**git基本命令和配置**
自报家门：$ git config --global user.name "Your Name"
					$ git config --global user.email "email@example.com"

将目录变为Git可以管理的仓库：git init
将文件添加到暂存区：git add < file >
将文件提交到仓库：git commit -m  < message >
查看当前仓库状态：git status
查看不同点：git diff
查看版本日志：git log
查看版本日志（简约版）：git log --pretty=oneline
回退版本：git reset --hard HEAD^(回退上一个版本，如回退多个版本，例：HEAD~100，回退100个版本)
回退指定版本：git reset --hard <版本号>
查找版本号(查看命令历史)：git reflog
查看工作区和版本库里面最新版本的区别： git diff HEAD -- < file>
丢弃工作区的修改：git checkout -- < file>
撤销暂存区的修改: git reset HEAD < file>
工作区删除文件：rm < file>
版本库中删除文件 ：先git rm  < file> 再 git commit
误删工作区文件恢复：git checkout -- < file>(类似一键还原)

**远程仓库**
第一步，创建ssh key ，在用户主目录下，找到.ssh目录，创建ssh key：ssh-keygen -t rsa -C "youremail@example.com" ，然后一路回车即可，在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
第二步，登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”，你就应该看到已经添加的Key，

**添加远程库**
首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库，
关联仓库：git remote add origin git@github.com:< git账户名>/仓库名.git     (origin为远程库名字，习惯性命名，可改)
第一次将本地库的所有内容推送到远程库上：git push -u origin master
非第一次将本地库的所有内容推送到远程库上：git push origin master
克隆一个本地库：git clone git@github.com:< git账户名>/仓库名.git

**分支**
创建和切换dev分支：git checkout -b dev
创建分支：git branch < name>
查看当前分支：git branch
切换分支：git checkout master（master为分支名）

快速合并分支的条件：
![快速合并分支条件](https://img-blog.csdnimg.cn/20190108170502645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMjUxNzg5,size_16,color_FFFFFF,t_70)
合并指定分支(快速合并模式)：git merge dev（当前分支为master，把dev分支的工作成果合并到master分支上，git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的）
合并完成后，删除dev分支：git branch -d dev

分支冲突：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019010817071472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMjUxNzg5,size_16,color_FFFFFF,t_70)
执行快速合并后需打开文件手动解决冲突
用带参数的git log也可以看到分支的合并情况：git log --graph --pretty=oneline --abbrev-commit
查看分支合并图：git log --graph

**分支管理策略**
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
下面我们实战一下--no-ff方式的git merge：git merge --no-ff -m "merge with no-ff" dev（dev为分支名）
不使用Fast forward模式，merge后就像这样：
![不使用Fast forward模式](https://img-blog.csdnimg.cn/20190108192512612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMjUxNzg5,size_16,color_FFFFFF,t_70)

**分支策略**
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：
![团队合作的分支](https://img-blog.csdnimg.cn/20190108192744976.png)

Git分支十分强大，在团队开发中应该充分应用。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

**Bug分支**
暂存工作现场：git stash
查看缓存的现场：git stash list
恢复现场（不删除stash）：git stash apply
恢复现场（同时删除stash）：git stash pop
恢复指定的缓存：git stash apply stash@{0}（缓存名）

强制销毁未保存的分支：git branch -D < name>

**多人协作**
查看远程库信息：git remote
查看远程库详细信息：git remote -v
推送分支：git push origin < name>

并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

master分支是主分支，因此要时刻与远程同步；

dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

抓取分支：git clone < 路径>

你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，
于是他用这个命令创建本地dev分支：git checkout -b dev origin/dev

多人协作的工作模式通常是这样：
首先，可以试图用git push origin < branch-name>推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin  < branch-name>推送就能成功！
如果git pull提示no tracking information，则*说明本地分支和远程分支的链接关系没有创建*，
用命令git branch --set-upstream-to < branch-name> origin/< branch-name>。

**标签管理**
概念：发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
tag（标签）就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

创建标签：git tag < tag-name>
查看所有标签：git tag
给指定版本建标签：git tag < tag-name> < commit-id>
查看指定标签详细信息：git show < tag-name>
可以指定标签信息：git tag -a < tagname> -m "blablabla..."
删除标签：git tag -d < tagname>（创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除）
推送某个标签到远程：git push origin < tagname>
推送全部尚未推送到远程的本地标签：git push origin --tags

删除已经推送到远程的标签，先删除本地的标签git tag -d  < tagname>，
再从远程删除git push origin :refs/tags/< tagname>

**忽略特殊文件**
有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...，有强迫症的童鞋心里肯定不爽。好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：
忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

在.gitignore文件添加类似以下内容即可自动忽略相应文件：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```
最后一步就是把.gitignore也提交到Git，就完成了！当然检验.gitignore的标准是git status命令是不是说working directory clean。
使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。
有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了：

如果你确实想添加该文件，可以用-f强制添加到Git：git add -f App.class
或者你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，
可以用git check-ignore命令检查：git check-ignore -v App.class
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理

**配置别名**
有没有经常敲错命令？比如git status？status这个单词真心不好记。
如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。
我们只需要敲一行命令，告诉Git，以后st就表示status：
git config --global alias.st status

当然还有别的命令可以简写，
很多人都用co表示checkout，ci表示commit，br表示branch：
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch

甚至还有人丧心病狂地把lg配置成了：
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
**配置文件**
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中：

```
 cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1
```
别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中
配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。