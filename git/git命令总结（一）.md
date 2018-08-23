# git 命令总结

好久没有写文啦~凡事重在坚持！赶紧写起来~

最近用git指令比较多，之前对git指令比较模糊，借此机会，总结一下git常用命令。尽量覆盖全面，不太常用的指令会以参考链接的形式给出~



## 一图解所有

借用大神一张图来说明：

![](https://raw.githubusercontent.com/WarpPrism/Blog/master/imgs/34/git.jpg)



+ **首先我们弄清楚几个概念：**
  + **workspace**:工作区，也就是我们写代码的地方，它持有实际的文件；
  + **index**：也可以叫做stage，暂存区，它实际上就是一个包含文件索引的目录树 ，当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新。
  + **local repository**：本地仓库，我们使用“git commit”时，实际是把代码提交到了本地仓库；
  + **remote repository**：远端仓库，当我们使用“git push”时，才将我们的本地代码提交到了远端的仓库，比如github上。
+ 什么是origin？
  +  远程库的名字就是origin，这是Git默认的叫法，也可以改成别的。


## git 常用命令

#### 检出仓库：

+ 本地仓库的克隆版本：`git clone /path/to/repository `；
+ 克隆远端服务器仓库：`git clone username@host:/path/to/repository `;



#### 将本地仓库关联到远程仓库

+  比如要加入远程仓库的名字为 learngit:`git remote add origin 邮箱:用户名/learngit.git`



#### 添加与提交

+ 添加到缓存区：`git add <filename> `；
+ 提交到本地仓库：`git commit -m "代码提交信息" `



#### 推送改动

+ 比如要提交到master：`git push origin master `
+ 如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加： `git remote add origin <server> `



#### 分支

+ 创建仓库的时候，*master* 是“默认的”。在其他分支上进行开发，完成后再将它们合并到主分支上。
+ 创建一个feature_x分支，并且切换到feature_x分支上：`git checkout -b feature_x`；
+ 删除分支：`git branch -d feature_x`;
+ 将分支推送到远端仓库：``git push origin <branch>` `;



#### 替换本地改动

+ 使用如下命令替换掉本地改动，：`git checkout -- <filename>`；此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到缓存区的改动，以及新文件，都不受影响。
+ 丢弃你所有的本地改动与提交，可以到服务器上获取最新的版本并将你本地主分支指向到它：`git reset --hard origin/master`。



## git协同开发常用命令

+ 显示所有远程连接的库：`git remote`;
+ 创建url远程库的链接，之后可以用<name>来代表<url>:`git remote add <name> <url>`;
  + 这里用到的url链接可以时ssh链接也可以是时http链接。http链接允许对库进行**匿名但只读**的访问，在这种状态下时不可能把commit推送到中央库的（比如http://host/path/to/repo.git，无用户名匿名访问）。而使用ssh链接的时候是**可读写**的，但是不允许匿名（比如ssh:[//user@host/path/to/repo.git](mailto://user@host/path/to/repo.git)）。
+ 把远程库的commit输入到本地仓库：`git fetch`;
+ 查看远程分支：`git branch -r`;
+ 合并本地与远程分支，如果有冲突要手动修改：`git merge origin/<current-branch>`;
+ git fetch和git merge合并在一起的快捷方式：`git pull <remote>`;



## 参考链接

git简易指南：http://www.bootcss.com/p/git-guide/

git协同开发：http://www.cnblogs.com/arkenstone/p/6339042.html

github文章借鉴：https://github.com/WarpPrism/Blog/issues/34

gitbook：http://gitbook.liuhui998.com/index.html

