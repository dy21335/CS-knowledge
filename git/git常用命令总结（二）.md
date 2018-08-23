# git 常用命令

以下为git常用的命令的总结，方便大家查阅与参考：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftlwob0ytmj20c30czq3e.jpg)



以下文章中，远程仓库表示git clone 来源的仓库，上游仓库是git add <remote>的仓库；

## 概念解析

借用图来说明：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftlwr7s79aj20dd0cnjrn.jpg)

- **几个概念：**

- - **workspace**:工作区，也就是我们写代码的地方，它持有实际的文件；
  - **index**：也可以叫做stage，暂存区，它实际上就是一个包含文件索引的目录树 ，当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新。
  - **local repository**：本地仓库，我们使用“git commit”时，实际是把代码提交到了本地仓库；
  - **remote repository**：远端仓库，当我们使用“git push”时，才将我们的本地代码提交到了远端的仓库，比如github上。

- origin

- - 远程库的名字就是origin，这是Git默认的叫法，也可以改成别的。

-                                

##  常用命令

#### 克隆仓库：

克隆本地仓库：

```
git clone /path/to/repository
```

克隆远端服务器仓库：

```
git clone username@host:/path/to/repository
```

                                                

#### 设置上游仓库

设置上游仓库，就可对上游仓库的代码进行fetch等操作，多用于协同开发；

```
git remote add <name> <url>
```

url是远程仓库的的地址，这么设置后，可用name代替url；

                           

#### 查看上游仓库

查看所有上游仓库分支：

```
git branch -r
```

查看所有上游仓库名字：

```
git remote
```

查看所有上游仓库名字和git地址：

```
git remote -v
```

删除上游仓库：

```
git remote rm <remote>
```

替换已有的上游分支的url:

```
git remote set-url <remote> <url> //这会使原本关联的remote的url替换成新的url
```



                       

#### 分支

创建仓库的时候，*master* 是“默认的”。在其他分支上进行开发，完成后再将它们合并到主分支上。

创建一个feature_x分支，并且切换到feature_x分支上：

```
git checkout -b feature_x //不加-b则是切换到某一分支上
```

删除分支：

```
git branch -d feature_x
```

查看当前所处分支：

```
git branch
```

查看包括上游仓库与远程仓库所有分支：

```
git branch --all
```



                                   

#### 拉取更新

```
git fetch //拉取远程仓库更新到本地仓库，默认为git clone的那个仓库；
git fetch <name> <branch> //拉取远程仓库名为<name> 的 某一分支<branch>上的代码，<branch>可省略；
```

如果我们有多个remote都需要更新，可以使用：

```
git remote update
//或者
git fetch --all
```



#### 合并更新

如果有冲突，则要手动合并；

在合并之前可以选择性执行如下操作：

```
git diff <name>/<target_branch>//比较本地工作区与上游仓库特定分支的差别；
git stash //保存当前更改到缓存区
git merge <name>/<target_branch> //将上游仓库的特定分支代码合并到本地；
git stash pop //将缓存区代码提出到本地，并修改冲突；
```

快捷操作：

 ```
git pull //相当于git fetch和git merge的两部操作  
 ```

 强制更新远程仓库的代码到本地（慎用）：

```
git merge --rebase 
```

rebase与merge区别参考链接：https://hackernoon.com/git-merge-vs-rebase-whats-the-diff-76413c117333

                                              

#### 存储到缓存区以及拉取缓存区内容

缓存区存放工作区文件改动内容，当执行git stash后，所有改动被放到缓存区，并且工作区恢复到没有改动之前。

```
git stash
git stash pop
```

          

#### 添加到暂存区

添加到暂存区（index）：

```
git add <filename>；//添加单个文件；
git add . //git 2.0 以上添加所有改动；
git add -u //添加修改某些文件和删除某些文件的改动，但不添加新建文件的改动；
```

                    

#### 提交和推送改动

提交暂存区的改动到本地仓库：

```
git commit -m "代码提交信息"
```

推送本地仓库改动到远程仓库，比如要提交到master分支上：

````
git push origin master
````

强制推送（慎用）：

```
git push --force
```

                           

#### 查看当前状态

列出当前目录所有还没有被git管理的文件和被git管理且被修改但还未提交(git commit)的文件:

```
git status
```



#### 查看记录

查看所有commit记录:

```
git log
```

git log常用选项：

| 选项           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| -p             | 按补丁格式显示每个更新之间的差异                             |
| –stat          | 显示每次更新的文件修改统计信息                               |
| –shortstat     | 只显示 –stat 中最后的行数修改添加移除统计                    |
| –name-only     | 仅在提交信息后显示已修改的文件清单                           |
| –name-status   | 显示新增、修改、删除的文件清单                               |
| –abbrev-commit | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符              |
| –relative-date | 使用较短的相对时间显示（比如，“2 weeks ago”）                |
| –graph         | 显示 ASCII 图形表示的分支合并历史                            |
| –pretty        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式） |

 限制输出长度

- 除了用 `-n` 来限制输出 log 的条数，还可以用 `--since` 和 `--until` 按照时间作限制。

- 用 `--author` 选项显示指定作者的提交，用 `--grep` 选项搜索提交说明中的关键字。


#### 回滚

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftge1ell6oj212c0azgmi.jpg)

如果想回滚到某一commit状态，使用git log 查看commit记录，然后执行如下命令：

```
git reset <commit>
//执行完这一步，以后想要push，就要执行如下命令：
git push --force //强制将当前commit覆盖以前的回滚点之后的commit
```

                                      

####  比较提交

比较当前工作区与暂存区之间的差异：

```
git diff
```

比较暂存区与本地仓库之间的差异：

```
git diff --cached
```

比较当前工作区与本地仓库之间的差异：

```
git diff Head
```

比较当前工作区与另外一个分支的差别，比如分支为test：

```
git diff test
```

比较当前工作区与暂存区有哪些文件被改动，改动了多少行：

```
git diff --stat
```

比较两个分支的操作：

```
git diff <source_branch> <target_branch>
```

               

#### 交互式的rebase

列出即将push的所有git commit ，编辑这些commit，可以对这些commit进行合并，修改，丢弃，编辑，拆分操作：

```
git rebase -i  //会打开编辑器，可修改commit记录
git rebase -i <after-this-commit> //当前分支中的所有提交
```

rebase 参考链接https://git-scm.com/docs/git-rebase；

rebase 拆分commit参考链接：http://www.codebelief.com/article/2017/03/git-advanced-usage-split-specific-commit-in-the-past/

                                            
request


## 参考链接

git简易指南：http://www.bootcss.com/p/git-guide/

git协同开发：http://www.cnblogs.com/arkenstone/p/6339042.html

github文章借鉴：https://github.com/WarpPrism/Blog/issues/34

gitbook：http://gitbook.liuhui998.com/index.html