# 如何优雅地pull request

本文章主要用于阐述pr流程，以及可能遇到的情况和解决方案，帮助大家更好地协作开发。



> Pull Request请求通常由使用共享存储库协作的团队和组织使用。 
>
> Github上的许多开源项目使用pull request请求来管理来自贡献者的更改，因为它们提供了一种方式来通知项目维护者其他贡献者所做的更改，并且在合并到主分支之前，能对代码进行review和对此进行讨论。



+ 主要有两种进行pull request的工作流程；
  + 对一个forked仓库进行pull request；
  + 对一个仓库里的分支进行pull request；

本文主要对第一种方式进行讲解。

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftlwu873pyj20bo0gkwfi.jpg)

[TOC]



## 1. Fork仓库

1.  将需要参与的项目仓库fork到自己的github中；

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftjvvtnnedj20d8030glj.jpg)



## 2. git clone这个fork的项目

1.  将自己的github中，找到fork下来的项目，git clone 到本地。



## 3. 添加上游仓库

1.  接着上一步，在该项目中，执行以下操作，将上游仓库连接到本地仓库，即我们fork的地址：

```
git remote add <name> <url>
```

2.  其他辅助功能：

```
git remote -v  //查看所有上游仓库名字和git地址：
```



## 4. 保持与上游仓库的同步

1. 更新 "指定" remote 底下的分支：

   ```
   git fetch <name> <branch>
   ```

2. 上面这个指令，一次只能更新一个remote，如果我们有多个remote时，可以使用：

   ```
   git remote update
   //或者
   git fetch --all
   ```

3. 当我们使用pull命令拉取上游分支的最新代码时，如果本地分支落后于上游分支，会产生一个新的merge commit 多余信息，因此建议使用：

```
git pull --rebase <remote> <branch>
//等同于以下两条命令
git fetch <remote> <branch>
git rebase <remote>/<branch>
```

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl1edwf8yj20hk0c9js1.jpg)



## 5. 提出issue

> issue对你的项目，提供了一种很好的方式去追踪，加强，修复bug。它有点像是邮件，但是它可以被一起讨论。

#### 对于项目维护者

1. 建议项目维护者维护项目的issue模板，贡献者们按着这个模板提issue；

2. 有两种方式创建issue模板：

   - 通过github创建：https://help.github.com/articles/creating-issue-templates-for-your-repository/  

   - 第二种手动创建一个issue模板：https://help.github.com/articles/manually-creating-a-single-issue-template-for-your-repository/

     **示例：**

     （1）在项目中，新增一个bug模板和一个feature模板文件:

     ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl2tua6uyj207e02lwed.jpg)

     其中bug_request模板的内容如下：

     ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl2ufkm24j20et0a4q3b.jpg)



     （2）此后，贡献则在github中打开issue可看到对应模板选项：

     ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl2wcrqdbj20lb08k3z1.jpg)

     选择feature request，便可根据要求填写：

     ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm9aljd5oj20lm0fdjs8.jpg)

   - 必须要在默认分支中创建issue模板。

#### 对于贡献者

1. **在new一个pull request之前，最好的做法是先提出一个issue，供大家讨论；**

2. 请在issue中参照issue规范，提供足够多的信息来帮助别人理解；

3. 请为自己的issue添加对应的标签，以便可以方便对issues进行分类和过滤：

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftjsk7zfaij207o09ut8s.jpg)

   

   - Assigns是受委托人——负责推进issue的人；
   - 一个issues可以有多个Label；
   - Milstone对应于项目，功能或时间段的issue组：比如版本号，具体截止时间，重构新功能等。

4. 可以对提交的issue进行订阅，得知issue的推进情况：

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftjsisotz4j207y03ajra.jpg)



## 6. 生成一个新分支

1. 建议贡献者创建一个自己的新分支，在该分支上进行操作，最后通过pull request方式合并到主分支上。

```
git checkout -b feature_x //不加-b则是切换到某一分支上，加上-b就是创建且切换
```

2. 最后push的时候，要使用：

   ```
   git push --set-upstream origin <分支名>
   ```

   将新分支提交到远程仓库中。

   如果上游仓库也需要建立这个分支，这可以使用：

   ```
   git push --set-upstream <remote> <分支名>
   ```


## 7. 提交commit信息

在对项目作出更改后，我们需要生成commit来记录自己的更改。以下是Angular对commit格式的规范：

### (1) 格式

提交信息包括三个部分：`Header`，`Body` 和 `Footer`。

```
<Header>

<Body>

<Footer>
```

其中，Header 是必需的，Body 和 Footer 可以省略。

#### 1> Header

Header部分只有一行，包括俩个字段：`type`（必需）和`subject`（必需）。

```
<type>: <subject>
```

**type**

type用于说明 commit 的类别，可以使用如下类别：

- feat：新功能（feature）
- fix：修补bug
- doc：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

**subject**

subject是 commit 目的的简短描述。

- 以动词开头，使用第一人称现在时，比如改变，而不是改变了。
- 结尾不加句号（。）

#### 2> Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

**注意：**应该说明代码变动的动机，以及与以前行为的对比。

#### 3> Footer

	Footer 部分应该包含：(1)Breaking Changes;  (2)关闭issue；

	**Breaking Changes**：

	如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。这种使用较少，了解即可。

	**Issue部分：**

+ 通过commit关联issue：

  如果当前提交信息关联了某个issue，那么可以在 Footer 部分关联这个 issue：

  ```
  issue #2
  ```



  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm7sa85ohj20y205s759.jpg)



  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm7ss51quj20tc064t9f.jpg)

+ 通过commit关闭issue，当提交到**默认分支**时，提交信息里可以使用 `fix/fixes/fixed` , `close/closes/closed` 或者 `resolve/resolves/resolved`等关键词，后面再跟上Issue号，这样就会关闭这个Issue：

```
Closes #1
```

	注意，如果不是提交到默认分支，那么并不能关闭这个issue，但是在这个issue下面会显示相关的信息表示曾经想要关闭这个issue，当这个分支合并到默认分支时，就可以关闭这个issue了。

	如下图所示，为向默认分支提交关闭issue的commit：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm6xeazzcj20q604mjs6.jpg)



![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm75iy1zcj20ue0ekwgg.jpg)

	而在别的分支想要关闭时：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm7qd3h33j20v605i0th.jpg)



#### 4> 例子

下面是一个完整的例子：

```
feat: 添加了分享功能

给每篇博文添加了分享功能

- 添加分享到微博功能
- 添加分享到微信功能
- 添加分享到朋友圈功能

Issue #1, #2
Closes #1
```

上面的提交信息应该能够解释自己的意思了。



## 8. 整合commit信息，保持commit信息的干净度

### (1) 通过git rebase -i清洗commit记录

我们可以通过rebase  -i完成以下目的：

+ 编辑以前的commit信息；
+ 将多个commit信息整合到一个；
+ 删除commit信息。

注意这些commit都是还未被push的信息；



#### 使用rebase -i进入交互模式

rebase 有6个选项可以选择：

+ pick ： pick意味着采用该commit；
+ reword：reword选项可以修改commit信息；
+ edit：可以修改commit的提交信息，不同于reword，这个选项可以暂停rebase过程，使得你可以添加更多的commit信息，这允许您将大型提交拆分为较小的提交，或者删除提交中所做的错误更改；
+ squash：此命令允许您将两个或多个提交组合到一个提交中。提交被压缩到它上面的提交中。 Git让您有机会编写描述这两个更改的新提交消息。
+ fixup：这与squash类似，但要合并的提交会丢弃其消息。提交只是简单地合并到它上面的提交中，并且先前的提交消息用于描述这两个更改。
+ exec：这允许您针对提交运行任意shell命令。

#### 例子讲解

以下是一个squash的示例：

>  如图，有三个commit记录：

>  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftkyxgbwejj20g808haaq.jpg)

> 执行`git rebase -i`：

>  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftkyzxpgpyj20i60efjsu.jpg)

>  进入编辑界面，如果我们是想要合并两个commit记录则可以进行如下编辑：

> ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftkz69di7qj207a023mx3.jpg)

> ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftkz7f1ncxj20er03uglm.jpg)

>  commit记录已经变为：

>  ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm8bfz6soj20f605qjro.jpg)



### (2) 撤销rebase 操作

1. 执行`git reflog`找到commit ID；
   + reflog可以查看从本地仓库创建之日起，本地所进行的与项目更改有关的操作，比如说commit，clone，rebase等操作。`git log`是查看当前版本库及其之前的所有commit。
   + `git reflog`默认其实是`git reflog show Head`;可以使用`git reflog show --all`来查看所有分支的情况；
2. 执行`git reset --hard <commit ID>`;



### (3) 回退到某一commit状态

当想撤回某一commit记录时，可以采用回退的方式，但是注意这种方式也将使你的后续更改丢失。

1. 执行`git log`命令，查看commit记录：

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm99sg5l8j20ef052weu.jpg)

   如上图，可以看到commit的哈希值；	

   如果增加`--pretty=oneline`参数，则可以只显示版本号和提交时的备注信息。

2. 执行`git reset --hard commit_id`  ：

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftge1ell6oj212c0azgmi.jpg)

   例如我们需要回退到`新增MVC模式`的上一个版本，则执行：

   ```
   git reset a5f2a27f02d32b666e01c24ed2218598db57a183 //此为默认方式，不带任何参数的git reset它回退到某个版本，只保留源码，回退commit和index信息
   ```

   如果是--hard参数，则彻底回退到某个版本，本地工作区也会变为上一个版本的内容：

   ```
   git reset --hard a5f2a27f02d32b666e01c24ed2218598db57a183
   ```

   如果是--soft参数，会保留工作区和暂存区的记录，在下次可以直接commit：

   ```
   git reset --soft a5f2a27f02d32b666e01c24ed2218598db57a183
   ```

   这种情况下在push时，要执行：

   ```
   git push --force 
   ```




## 9. Push到远程仓库

1. 在push之前，应该再pull一次上游仓库，将本地commit与远程commit对比，如果远程仓库已经有个新的更新，并且与本地仓库产生冲突，那么就需要先解决冲突，接着在git add && git commit；

   



## 10.  new 一个 pull request

1. 在自己的项目中new 一个 pull request，选定哪一个分支要合并到哪一个分支：

![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftm9bvknh9j20yt04874n.jpg)

2. 选择审阅者，委托人，标签等信息；

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl1po19frj20740azjrh.jpg)


## 11. 审阅者进行审阅

1. 如果新增代码不需要运行，审阅者审阅后可进行是否merge的评定；

2. 代码审阅者可以将该pull request的来源仓库设为上游仓库，拉取被审阅的代码，再运行，以运行结果判定是否merge；

3. 有三个merge行为，可根据具体情况选择：

   ![](http://ww1.sinaimg.cn/large/ec07bbd2gy1ftl1ydd1orj209o083t92.jpg)




## 其他情况

1. 丢弃本地所有修改（已经commit）；

   ```
   git reset Head^   //可加参数--hard或者--soft 
   ```

2. 取消已经暂存的文件，即，撤销先前"git add"的操作

   ```
   git reset Head 
   ```

   (未被追踪的文件不会得到更改，也就是没有执行git add的文件)

3. 将本地状态回退到和远程的一样：

   ```
   git fetch origin/master
   git reset origin/master //可加参数--hard或者--soft
   ```

4. 查看某次commit记录具体内容：

   ```
   git show <commit ID> 
   //也可以用
   git show Head^^
   ```

5. cherry-pick

   cherry-pick 就是挑选一个我们需要的 commit 进行操作。它可以用于将在其他分支上的 commit 修改，移植到当前的分支，但是移植的只是一个副本，会生成一个新的commit记录。

   ```
   1.切换到 develop 分支。
   2.通过 git log,找到 C 的 SHA1 值。
   3.通过 git cherry-pick <C的SHA1> ，将 C 的修改内容合并到当前内容分支 develop 中。
   4.若无冲突，过程就已经完成了。如果有冲突，按正常冲突解决流程即可。
   ```

    关于cherry-pick的其他操作可以查看后面给出的参考链接。

6. 使用changlog打印commit信息（前提是按照angular规范书写commit信息）

   ```
   npm install -g conventional-changelog-cli //安装全局包；
   conventional-changelog -p angular -i CHANGELOG.md -s -r 0 //第一次使用时，执行此命令，会生成一个CHANGELOG.md文件
   conventional-changelog -p angular -i CHANGELOG.md -s  //以上生成的更改日志基于自上一个semver(语义化版本)标记以来的提交。
   ```

   一般在发包（release）的时候打印changlog信息，changelog实际上是release的一个步骤之一。



7. 结对编程：

   多人在同一分支上开发互相依赖的功能，比如A和B两人，都在分支feature-x上开发有互相依赖的新功能，以A为例进行步骤说明：

   ```
   //将B设为自己上游分支（A和Bfork了需要开发的项目）；
   git remote add B <url>
   //再根据需要，拉取B的代码
   git pull --rebase B <branch>
   ```

8. 更多操作指令请查看另一篇git常用命令文档。

## 参考链接

pr：https://www.thinkful.com/learn/github-pull-request-tutorial/#Finding-a-Project-to-Contribute-to

commit&changelog规范：http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html

angular规范：https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#

github issue API：https://developer.github.com/v3/issues/#create-an-issue

git reflog：https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog

git rebase : https://help.github.com/articles/about-git-rebase/

https://www.cnblogs.com/dracohan/p/5241511.html

issue模板：https://github.com/devOpenDocerPartner/base-component/pull/47/commits/63ddd12e882424ef268a0b210fbd513e9b4b2adb

commit中的issue：https://help.github.com/articles/closing-issues-using-keywords/

changelog：https://www.npmjs.com/package/conventional-changelog-cli

约定式提交：http://conventionalcommits.org/lang/zh-Hans

Cherry-pick：https://blog.csdn.net/qq_32452623/article/details/79449534

