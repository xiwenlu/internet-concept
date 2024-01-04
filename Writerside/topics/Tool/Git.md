# git

## Git命令
git branch 查看本地所有分支     
git status 查看当前状态     
git commit 提交     
git branch -a 查看所有的分支     
git branch -r 查看远程所有分支     
git commit -am "init" 提交并且加注释     
git remote add origin git@192.168.1.119:ndshow     
git push origin master 将文件给推到服务器上     
git remote show origin 显示远程库origin里的资源     
git push origin master:develop     
git push origin master:hb-dev 将本地库与服务器上的库进行关联     
git checkout --track origin/dev 切换到远程dev分支     
git branch -D master develop 删除本地库develop     
git checkout -b dev 建立一个新的本地分支dev     
git merge origin/dev 将分支dev与当前分支进行合并     
git checkout dev 切换到本地dev分支     
git remote show 查看远程库     
git add .     
git rm 文件名(包括路径) 从git中删除指定文件     
git clone git://github.com/schacon/grit.git 从服务器上将代码给拉下来     
git config --list 看所有用户     
git ls-files 看已经被提交的     
git rm [file name] 删除一个文件     
git commit -a 提交当前repos的所有的改变     
git add [file name] 添加一个文件到git index     
git commit -v 当你用－v参数的时候可以看commit的差异     
git commit -m "This is the message describing the commit" 添加commit信息     
git commit -a -a是代表add，把所有的change加到git index里然后再commit     
git commit -a -v 一般提交命令     
git log 看你commit的日志     
git diff 查看尚未暂存的更新     
git rm a.a 移除文件(从暂存区和工作区中删除)     
git rm --cached a.a 移除文件(只从暂存区中删除)     
git commit -m "remove" 移除文件(从Git中删除)     
git rm -f a.a 强行移除修改后文件(从暂存区和工作区中删除)     
git diff --cached 或 $ git diff --staged 查看尚未提交的更新     
git stash push 将文件给push到一个临时空间中     
git stash pop 将文件从临时空间pop下来

---
git remote add origin git@github.com:username/Hello-World.git     
git push origin master 将本地项目给提交到服务器中

---
git pull 本地与服务器端同步

---

git push (远程仓库名) (分支名) 将本地分支推送到服务器上去。     
git push origin serverfix:awesomebranch

---

git fetch 相当于是从远程获取最新版本到本地，不会自动merge     
git commit -a -m "log_message" (-a是提交所有改动，-m是加入log信息) 本地修改同步至服务器端 ：     
git branch branch_0.1 master 从主分支master创建branch_0.1分支     
git branch -m branch_0.1 branch_1.0 将branch_0.1重命名为branch_1.0     
git checkout branch_1.0/master 切换到branch_1.0/master分支     
du -hs     
git branch 删除远程branch     
git push origin :branch_remote_name     
git branch -r -d branch_remote_name

---

初始化版本库，并提交到远程服务器端     
mkdir WebApp     
cd WebApp     
git init 本地初始化     
touch README     
git add README 添加文件     
git commit -m 'first commit'     
git remote add origin git@github.com:daixu/WebApp.git     
增加一个远程服务器端     
上面的命令会增加URL地址为'git@github.com:daixu/WebApp.git'，名称为origin的远程服务器库，以后提交代码的时候只需要使用 origin别名即可

## git和svn区别
svn和git最大的几个区别要点，svn必须要有服务端，网络能连上服务端才能提交和更新，git不需要，每一台装了git的电脑都是服务端，各台电脑之间可以相互同步和推送，而提交不需要网络就可以提交到本地的git库里。     
对于吧友们来说，这样的好处就是，如果要分享代码，不需要打个压缩包传来传去，也不需要找个服务器搭个svn来共享，现在oschina，csdn等网站都提供了免费的git服务器，大家注册个帐号，提交下去，把地址发给别人就可以了，而免费的svn服务器，几乎是不存在的。而且svn因为服务端是单一的，一旦服务器坏了，整个版本库的历史记录就没有了，也没法再回滚，git每台电脑都是服务端，只要两台电脑做过同步，任何一台坏了，另一台还保有着所有的历史记录，仍然可以提交更新回滚，不怕代码历史丢失。       
相同：     
能记录文件的所有更改记录。这样是为了大量更改后，但是最后觉得还是原来的版本代码好，可以有记录回到过去，而不用采用 Copy 旧代码另存为某文件，然后某个时间从大量文件中找你需要的历史记录，版本控制帮我们做到了历史记录的存储，可以方便地查询及回滚到过去的某一版本。     
不同：
git和其他版本控制系统（如 CVS）有不少的差别，git本身关心文件的整体性是否有改变，但多数的 CV S或 Subversion 系统则在乎文件内容的差异。因此git更像一个文件系统，直接在本机上获取数据，不必连接到主机端获取数据。      
git 是用于Linux内核开发的版本控制工具。与CVS、Subversion(SVN) 一类的集中式版本控制工具不同，它采用了分布式版本库的作法，不需要服务器端软件，就可以运作版本控制，使得源代码的发布和交流极其方便。git的速度很快，这对于诸如Linux内核这样的大项目来说自然很重要。git最为出色的是它的合并追踪（merge tracing）能力。      
SVN 是集中式或者有中心式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就纳闷了。       
Git 是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了

SVN优缺点        
优点：
1. 管理方便，逻辑明确，符合一般人思维习惯。
2. 易于管理，集中式服务器更能保证安全性。
3. 代码一致性非常高。
4. 适合开发人数不多的项目开发。              
   缺点：
1. 服务器压力太大，数据库容量暴增。
2. 如果不能连接到服务器上，基本上不可以工作，看上面第二步，如果服务器不能连接上，就不能提交，还原，对比等等。
3. 不适合开源开发（开发人数非常非常多，但是Google app engine就是用svn的）。但是一般集中式管理的有非常明确的权限管理机制（例如分支访问限制），可以实现分层管理，从而很好的解决开发人数众多的问题。

Git优缺点        
优点：
1. 适合分布式开发，强调个体。
2. 公共服务器压力和数据量都不会太大。
3. 速度快、灵活。
4. 任意两个开发者之间可以很容易的解决冲突。
5. 离线工作。

缺点：
1. 学习周期相对而言比较长。
2. 不符合常规思维。
3. 代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息。       



## svn

```Shell
#查看
svn list svn://www.baidu.com/资料
#检出
svn checkout svn://www.baidu.com/资料
```

