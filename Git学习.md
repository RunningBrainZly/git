##  Git学习

1.下载安装好git后

2.打开git bash

3.在git bash中执行  git  config --global user.name "自己的用户名"

​	git  config --global user.email "自己的邮箱"

4.新建一个空文件夹，最好路径中不要有中文

5.在这个文件夹中右键打开git bash，执行git init（成功后会出现一个.git文件）

6.在文件夹中创建一个test.txt文件 在里面随便写一些字符

7.在git bash 中执行git add test.txt  什么都不显示就说明成功了，然后执行git commit -m "本次提交的简介"

8.接下来继续修改一下test.txt文件   执行  git status查看效果，（他会提醒你文件修改后未提交）

​	git diff 查看修改了哪些地方

​	然后git add test.txt  

​	git commit -m "修改文件"

9.使用git log来查看我们之前都提交过哪些（加上--pretty=oneline会让信息一行显示）

​	其中标注head的是我们当前的版本

10.git reset head^ 回退到上一个版本(上上个版本就是head^^   上100个版本就是head~100)

​	如果你回退回来后悔了怎么办？

​		git reflog 会记录你的每次操作（然后找到你之前提交的版本commit id）

​		然后执行 git reset commitid  （这里的commitid只用写前几位就行，git会自己去找，但位数也不能太少，位数太少重复后git就不知到该用哪个了）



###  撤销修改和管理修改

把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

​	.git相当于是git版本库，版本库中有暂存区和分支区

​	git add相当于把文件添加到暂存区

​	git commit把暂存区中的文件提交到当前分支

git commit 只会提交暂存区存放的文件，之后修改的文件没有被放入暂存区是不会被提交的

![1641356071299](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641356071299.png)



11.删除文件，一般我们不用一个文件后会直接删除，使用git status查看后会显示删除了某某文件 使得工作区中和版本库中的文件不一致了就会提醒你

​	如果你要想删除版本库中以提交的，就  git rm 文件名，然后 git commit -m "操作介绍"来提交

另一种是你不小心删错了，版本库中还有这个文件，可以 git checkout -- 文件名 来恢复

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！





###  Git远程仓库

​	首先git远程仓库不单单指的是github网站，也可以自己搭建一个git服务器来存放git仓库，使每个工作者都在这个服务器中拉取和提交代码到这个服务器中。



由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容



为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。





###  Git本地仓库和远程Git仓库关联

​	git remote add origin 远程仓库地址

​	git remote add origin https://github.com/你的git用户名/git仓库名

![1641358122943](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641358122943.png)

​	如果设置了ssh就可以用ssh上传 

​	git remote add origin git@github.com:你github的用户名/git仓库名

![1641357977295](C:\Users\ZLY\AppData\Roaming\Typora\typora-user-images\1641357977295.png)

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：





###  把本地仓库推送（push）到Git远程仓库

​	git push -u origin master

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。



当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告:输入yes就好了



###  Git clone

​	git clone 远程仓库地址



###  Git分支管理

​	分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

​	现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。



####  创建切换并查看分支

​	创建分支 git branch 分支名

​	切换分支 git switch分支名

​	查看分支（带*号的表示当前分支） git branch

#### 合并分支

​	切换回主分支后合并  git merge 分支名（合并后就可以正常显示了）

####  删除分支

​	git branch -d 分支名

####  Bug分支

​	修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

####  分支冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

####  分支策略

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

####  开发一个新的模块时（防止开发完成后又要删除）

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

####  多人协作

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突

####  rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。



###  标签管理

意义

​	Git有commit，为什么还要引入tag？

​	“请把上周一的那个版本打包发布，commit号是6a5819e...”

​	“一串乱七八糟的数字不好找！”

​	如果换一个办法：

​	“请把上周一的那个版本打包发布，版本号是v1.2”

​	“好的，按照tag v1.2查找commit就行！”

​	所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

​	

####  创建标签

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

####  操作删除标签

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。