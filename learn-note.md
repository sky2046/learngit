# git learning

## git 的安装

Debian 或 Ubuntu
sudo apt-get install git

Windows
msysgit是Windows版的Git，http://msysgit.github.io/

安装完成后，还需要最后一步设置。
* git config --global user.email "liugenxian2015@gmail.com"
* git config --global user.name "liugenxian"

## 创建版本库

首先创建一个空目录

$ mkdir learngit  
$ cd learngit  
$ pwd  

初始化一个Git仓库

git init

添加文件到Git仓库，分两步：

* 第一步，使用命令 git add 注意，可以反复多次使用，添加多个文件；
* 第二部，使用命令 git commit 提交文件。

## 时光机穿梭

* 要随时掌握工作区的状态，使用 git status 命令。
* 如果 git status 告诉你有文件被修改过，用 git diff 可以查看修改内容。

## 版本回退

* HEAD 指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 git reset --hard commit_id
* 穿梭前，用 git log 可以查看提交历史，以便确定要回退到哪个版本。
* 要重返未来，用 git reflog 查看命令历史，以便确定要回到未来的哪个版本

## 工作区和暂存区

* 工作区(Working Directory):就是在本地电脑里能看到的目录，比如learngit目录。

* 版本库(Repository):工作区有一个隐藏目录".git"，这个不算工作区，而是Git 的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage(或者叫做index)的暂存区，还有Git为我们自动创建的第一个分支 master，以及指向master的一个指针叫HEAD。

* 把文件往 Git 版本库添加及提交的过程，即先把需要提交的修改文件通通放到暂存区，然后，一次性提交暂存区的所有修改。

## 管理修改

* Git 跟踪并管理的是修改，而非文件。
* 什么是修改？比如新增一行，删除一行，更改某些字符，创建一个新文件等都是修改。

* 每次修改，如果不 add 到暂存区，那就不会加入到 commit 中。

> 第一次修改 -> git add -> 第二次修改 -> git commit  
> 第一次修改 -> add -> 第二次修改 -> add -> commit  

## 撤销修改

git checkout -- readme.txt
> 把 readme.txt 文件在工作区的修改全部撤销，这里有两种情况：
> 一种是 readme.txt 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一
样的状态；
> 一种是 readme.txt 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存
区后的状态。
> 总之，就是让这个文件回到最近一次 git commit 或 git add 时的状态。

git reset HEAD readme.txt
> git reset 命令既可以回退版本，也可以把暂存区的修改回退到工作区。用HEAD表示最新的版本。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区，想丢弃修改，分两步，第一步用命令 git reset HEAD file, 就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

## 删除文件

删除也是一个修改操作。

git add test.txt
git commit -m "add test.txt"

rm test.txt

如果确实要从版本库中删除文件，命令 git rm test.txt
如果是删错了，版本库还有，可以恢复。命令 git checkout -- test.txt 只能恢复文件到最新版本，会丢失最近一次提交后你修改的内容。

## 远程仓库

Git 是分布式版本控制系统，同一个Git 仓库，可以分布到不同的机器上。
GitHub，提供Git仓库托管服务的网站。注册GitHub账户。

SSH key
> Using the SSH protocol, you can connect and authenticate to remote servers and services. With SSH keys, you can connect to GitHub without supplying your username or password at each visit.
> SSH keys serve as a means of identifying yourself to an SSH server using public-key cryptography and challenge-response authentication.

第1步：创建 SSH Key
ssh-keygen -t rsa -C "liugenxian2015@gmail.com"
第2步：登陆GitHub，Add SSH Key

## 添加远程库

git remote add origin git@github.com:sky2046/learngit.git
git push -u origin master

* 要关联一个远程库，使用命令 git remote add origin git@server-name:path/repo-name.git
* 关联后，使用命令 git push -u origin master 第一次推送master分支的所有内容
* 此后，每次本地提交后，只要有必要，就可以使用命令 git push origin master 推送最新修改。

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，没有联网也可以正常工作。

## 从远程库克隆

假设我们从零开发，最好的方式是先创建远程库，然后从远程库克隆。

* 登陆GitHub，创建一个新的库 gitskills
* Initialize this repository with a README.
* 用命令 git clone git@github.com:sky2046/gitskills.git 克隆到本地。

Git 支持多种协议，默认的 git:// 使用SSH(速度更快)，但也可以使用https协议，如 GitHub的地址 https://github.com/sky2046/gitskills.git

## 分支管理

创建一个属于自己的分支，别人看不到，你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

## 创建与合并分支

每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支。
刚开始，只有一条时间线，这个分支叫主分支(master分支)，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点。

每次提交，master 分支都会向前移动一步，随着你不断提交，master分支的线也越来越长。
当我们创建新的分支，比如dev时，Git 新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上。从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变。

怎么合并？比如dev上的工作完成了，就可以把dev合并到master上，最简单的方法，就是直接把master指向dev的当前提交。

合并完后，甚至可以删除dev分支，删除dev分支，就是把dev指针给删掉。

Git 鼓励大量使用分支：
* 查看分支：git branch
* 创建分支：git branch name
* 切换分支：git checkout name
* 创建+切换分支： git checkout -b name
* 合并某分支到当前分支： git merge name
* 删除分支： git branch -d name

## 解决冲突

准备新的feature1分支，如果master分支和feature1分支各自都有新的提交，这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就有可能产生冲突。
冲突产生了，必须手动解决冲突后再提交，才能合并。

Git 用<<<<<<<,=======,>>>>>>> 标记不同分支的内容。

用带参数的git log命令可以看到分支的合并情况。
```
$ git log --graph --pretty=oneline --abbrev-commit
*   d164f39 conflict fixed
|\
| * 55c473d AND simple
* | f55615a & simple
|/
* 107c544 branch test
* 81fa19a add test.txt
* 0bb9eb8 git tracks changes
* dbdd52c git tracks changes
* b4a2026 understand how stage works
* 2b69123 append GPL
* 0ccfa0c add distributed
* 5b807d1 wrote a readme file
```

当Git无法自动合并分支时，必须首先解决冲突，解决冲突后，再提交，再合并。
用git log --graph 命令可以看到分支合并图。

## 分支管理策略

基本原则：首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活。每个人都在dev分支上干活，每个人都有自己的分支，时不时往dev上合并，在关键节点，会往master上合并。

合并分支时，加上--no-ff参数可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

## Bug分支

每个bug都可以通过一个新的临时分支来进行修复，修复完合并分支，然后把临时分支删除。

当手头工作没有完成时，先把工作现场 git stash一下，然后去修复bug，修复完后，再git stash pop，回到工作现场。

* 储藏工作场景 git stash
* 创建临时分支，git checkout -b issue-101
* 修复bug后，提交。 git add readme.txt, git commit -m "fix bug 101"
* 合并。git merge --no-ff -m "merged bug fix 101" issue-101
* 查看工作现场。 git stash list
* 恢复工作现场。 git stash apply stash@{0} 或  git stash pop，回到工作现场。

## feature分支

开发一个新feature，最好新建一个分支。
如果要丢弃一个没有被合并过的分支，用命令 git branch -D name 强行删除。

## 多人协作

* 查看远程仓库信息 git remote -v
* 本地新建的分支如果不推送到远程，对其他人就是不可见的
* 从本地推送分支，命令 git push origin branch-name，如果推送失败，先pull远程的新提交
* 在本地创建和远程对应的分支，git checkout -b branch-name origin/branch-name， 本地和远程分支名字最好一致。
* 建立本地分支和远程分支的关联，git branch --set-upstream branch-name origin/branch-name
* 从远程抓取分支，使用git pull 如果有冲突，先处理冲突

## 标签管理

Git的标签是版本库的快照，是指向某个commit的指针。
分支也是指向commit的指针，但分支可以移动，标签不能移动。

* 命令 git tag name 用于新建一个标签，默认为HEAD，也可以指定commit id
* git -a tagname -m "info of tag" 可以指定标签信息
* git -s tagname -m "info of tag" 可以用PGP签名标签
* git tag 查看所有标签
* git show tagname 查看标签详细信息 
* git tag -d tagname 删除一个本地标签
* git push origin tagname 推送一个本地标签到远程参考
* git push origin --tags 一次性推送全部未推送过的本地标签
* git push origin :refs/tags/tagname 删除一个远程标签

## 使用 GitHub

* 在GitHub上，可以任意Fork开源仓库
* 自己拥有Fork的仓库的读写权限
* 可以推送pull request给官方仓库来贡献代码

## 自定义Git

* 显示颜色 git config --global color.ui true
* 忽略某些文件时，要编写.gitignore， .gitignore 文件本身要放到版本库里，可以对.gitignore做版本管理
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
```

* 配置别名。如：
```
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%C(bold blue)<%an>%Creset' --abbrev-commit"

```

## 搭建Git服务器

* 安装git
* 创建git用户
* 创建证书登录
* 初始化Git仓库
* 禁用shell登录
* 克隆远程仓库

***