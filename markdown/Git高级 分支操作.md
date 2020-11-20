## 什么是分支？

在入门篇我们简单地讲解了Git的基本使用方法。在高级篇呢，我们首先要讲解一下分支的使用方法和操作。

在开发软件时，可能有多人同时为同一个软件开发功能或修复BUG，可能存在多个Release版本，并且需要对各个版本进行维护。

所幸，Git的分支功能可以支持同时进行多个功能的开发和版本管理。

### 什么是分支？

分支是为了将修改记录的整体流程分叉保存。分叉后的分支不受其他分支的影响，所以在同一个数据库里可以同时进行多个修改。

![分叉的分支可以合并。](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_1_1.png)

**下面是使用分支进行作业的图示。**

为了不受其他开发人员的影响，您可以在主分支上建立自己专用的分支。完成工作后，将自己分支上的修改合并到主分支。因为每一次提交的历史记录都会被保存，所以当发生问题时，定位和修改造成问题的提交就容易多了。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_1_2.png)

### master分支

在数据库进行最初的提交后, Git会创建一个名为master的分支。因此之后的提交，在切换分支之前都会添加到master分支里。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_1_3.png)

## 分支的运用

在Git您可以自由地建立分支。但是，要先确定运用规则才可以有效地利用分支。

这里我们会介绍两种分支 (“Merge分支”和 “Topic分支” ) 的运用规则。

### Merge分支

Merge分支是为了可以随时发布release而创建的分支，它还能作为Topic分支的源分支使用。保持分支稳定的状态是很重要的。如果要进行更改，通常先创建Topic分支，而针对该分支，可以使用Jenkins之类的CI工具进行自动化编译以及测试。

通常，大家会将master分支当作Merge分支使用。

## Topic分支

Topic分支是为了开发新功能或修复Bug等任务而建立的分支。若要同时进行多个的任务，请创建多个的Topic分支。

Topic分支是从稳定的Merge分支创建的。完成作业后，要把Topic分支合并回Merge分支。

![特性分支的图示](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_2_1.png)

## 事前预备

首先建立一个新目录，并在里面建立一个空数据库。这里我们创建一个名为lmonkey的目录。

```
$ mkdir lmonkey
$ cd lmonkey
$ git init
Initialized empty Git repository in /Users/eguchi/Desktop/lmonkey/.git/
```

在lmonkey目录创建一个名为myfile.txt的档案，然后提交。

```
学习Git
```

```
$ git add myfile.txt
$ git commit -m "first commit"
[master (root-commit) a73ae49] first commit
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 myfile.txt
```

目前的历史记录是这样的。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_1_1.png)

## 建立分支

创建名为issue1的分支。

您可以通过branch命令来创建分支。

```
$ git branch <branchname>
```

创建名为issue1的分支。

```
$ git branch issue1
```

不指定参数直接执行branch命令的话，可以显示分支列表。 前面有*的就是现在的分支。

```
$ git branch
  issue1
* master
```

目前的历史记录是这样的。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_2_1.png)

## 分支的切换

若要切换作业的分支，就要进行checkout操作。进行checkout时，git会从工作树还原向目标分支提交的修改内容。checkout之后的提交记录将被追加到目标分支。

![分支的切换](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_3_1.png)

### HEAD

HEAD指向的是现在使用中的分支的最后一次更新。通常默认指向master分支的最后一次更新。通过移动HEAD，就可以变更使用的分支。

## 切换分支

若要在新建的issue1分支进行提交，需要切换到issue1分支。

要执行checkout命令以退出分支。

```
$ git checkout <branch>
```

切换到issue1分支。

```
$ git checkout issue1
Switched to branch 'issue1'
```

目前的历史记录是这样的。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_3_1.png)

**提示**

在checkout命令指定 -b选项执行，可以创建分支并进行切换。

```
$ git checkout -b <branch>
```

在切换到issue1分支的状态下提交，历史记录会被记录到issue1分支。在myfile.txt添加add命令的说明后再提交。

```
学习Git
add 把变更录入到索引中
```

```
$ git add myfile.txt
$ git commit -m "添加add的说明"
[issue1 b2b23c4] 添加add的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
```

目前的历史记录是这样的。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_3_2.png)

## stash 暂存

还未提交的修改内容以及新添加的文件，留在索引区域或工作树的情况下切换到其他的分支时，修改内容会从原来的分支移动到目标分支。

但是如果在checkout的目标分支中相同的文件也有修改，checkout会失败的。这时要么先提交修改内容，要么用stash暂时保存修改内容后再checkout。

stash是临时保存文件修改内容的区域。stash可以暂时保存工作树和索引里还没提交的修改内容，您可以事后再取出暂存的修改，应用到原先的分支或其他的分支上。

![stash](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_3_3.png)

### 有未提交的更改时切换分支

如果当前工作树中的有未提交的到数据库的内容

而此时又需要切换到其他的分支进行操作, 但是当前分支的工作又没有完成, 不能提交到数据库

如果直接切换到其他分支, 会带着当前分支未提交的内容一起切换到其他分支

开看一下, 修改myfile.txt的内容

```
学习Git
add 把变更录入到索引中
stash
```

切换到其他分支:

```
$ git checkout master
```

查看工作树状态:

```
$ git status
```

### 暂存未提交的修改

正确的做法是什么? 使用stash

首先回到issue1分支:

```
$ git checkout issue1
```

把当前分支的工作树中的更改内容暂存:

```
$ git stash
```

再次切换到其他分支, 并查看状态:

```
$ git checkout master
$ git status
```

可以看到, 其他分支中未提交的内容的, 并没有跟随过来了

### 恢复暂存区未提交的修改

此时, 再次回到issue1分支, 如果恢复之前暂存的内容呢?执行命令即可:

```
$ git checkout issue1
$ git stash pop
```

查看工作树状态:

```
$ git status
```

可以看到之前暂存的内容就恢复了, 可以继续开发了

最后, 如果不想要工作树中未提交的更改, 可以使用checkout命令撤销:

```
$ git checkout myfile.txt
```

查看工作树状态:

```
$ git status
```

工作树是干净的, 已经没有任何改动了


## 分支的合并

完成作业后的topic分支，最后要合并回merge分支。合并分支有2种方法：使用merge或rebase。使用这2种方法，合并后分支的历史记录会有很大的差别。

### merge

使用merge可以合并多个历史记录的流程。

如下图所示，bugfix分支是从master分支分叉出来的。

![分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_1.png)

合并 bugfix分支到master分支时，如果master分支的状态没有被更改过，那么这个合并是非常简单的。 bugfix分支的历史记录包含master分支所有的历史记录，所以通过把master分支的位置移动到bugfix的最新分支上，Git 就会合并。这样的合并被称为fast-forward（快进）合并。

![fast-forward合并](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_2.png)

但是，master分支的历史记录有可能在bugfix分支分叉出去后有新的更新。这种情况下，要把master分支的修改内容和bugfix分支的修改内容汇合起来。

![分叉分支後进行新的更新](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_3.png)

因此，合并两个修改会生成一个提交。这时，master分支的HEAD会移动到该提交上。

![结合了两个修改的合并提交](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_4.png)

**注意**

执行合并时，如果设定了non fast-forward选项，即使在能够fast-forward合并的情况下也会生成新的提交并合并。

![non fast-forward合并](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_5.png)

执行non fast-forward后，分支会维持原状。那么要查明在这个分支里的操作就很容易了。

## rebase

跟merge的例子一样，如下图所示，bugfix分支是从master分支分叉出来的。

![分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_6.png)

如果使用rebase方法进行分支合并，会出现下图所显示的历史记录。现在我们来简单地讲解一下合并的流程吧。

![使用rebase合并分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_7.png)

首先，rebase bugfix分支到master分支, bugfix分支的历史记录会添加在master分支的后面。如图所示，历史记录成一条线，相当整洁。

这时移动提交X和Y有可能会发生冲突，所以需要修改各自的提交时发生冲突的部分。

![使用rebase合并分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_8.png)

rebase之后，master的HEAD位置不变。因此，要合并master分支和bugfix分支，即是将master的HEAD移动到bugfix的HEAD这里。

![使用rebase合并分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_4_9.png)

### 注意

Merge和rebase都是合并历史记录，但是各自的特征不同。

**merge**

保持修改内容的历史记录，但是历史记录会很复杂。

**rebase**

历史记录简单，是在原有提交的基础上将差异内容反映进去。
因此，可能导致原本的提交内容无法正常运行。

您可以根据开发团队的需要分别使用merge和rebase。
例如，想简化历史记录，

- 在topic分支中更新merge分支的最新代码，请使用rebase。
- 向merge分支导入topic分支的话，先使用rebase，再使用merge。

## 合并分支

向master分支合并issue1分支的修改。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_3_2.png)

执行merge命令以合并分支。

```
$ git merge <commit>
```

该命令将指定分支导入到HEAD指定的分支。先切换master分支，然后把issue1分支导入到master分支。

```
$ git checkout master
Switched to branch 'master'
```

打开myfile.txt档案以确认内容，然后提交。

```
学习Git
```

已经在issue1分支进行了编辑上一页的档案，所以master分支的myfile.txt的内容没有更改。

```
$ git merge issue1
Updating 1257027..b2b23c4
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

master分支指向的提交移动到和issue1同样的位置。这个是fast-forward（快进）合并。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_4_1.png)

打开myfile.txt档案，确认内容。

```
学习Git
add 把变更录入到索引中
```

已添加「add 把变更录入到索引中」。

## 删除分支

既然issue1分支的内容已经顺利地合并到master分支了，现在可以将其删除了。

在branch命令指定-d选项执行，以删除分支。

```
$ git branch -d <branchname>
```

执行以下的命令以删除issue1分支。

```
$ git branch -d issue1
Deleted branch issue1 (was b2b23c4).
```

issue1分支被删除了。您可以用branch命令来确认分支是否已被删除。

```
$ git branch
* master
```

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_5_1.png)

> 注意：您必须离开你要删除的分支，才能删除这个分支。也就是不能删除当前所在的分支

**提示**

如果因一些原因不能删除分支, 可以使用-D参数强制删除分支:

```
$ git branch -D <branchname>
```

比如，您要删除的分支的历史家里和当前所在分支的历史纪录不一致的时候，就不能删除分支

会提示您，分支没有合并到当前分支

如果确定要删除分支， 可以使用 `branch -D` 强制删除

```
$ git checkout -b test
$ echo 111 >> myfile.txt
$ git add .
$ git commit -m 'text'
$ git checkout master
$ git branch -d test
error: The branch 'test' is not fully merged.
If you are sure you want to delete it, run 'git branch -D test'.
```

```
$ git branch -D test
```

## 并行操作

接下来，创建2个分支来尝试并行操作吧。

首先创建issue2分支和issue3分支，并切换到issue2分支。

```
$ git branch issue2
$ git branch issue3
$ git checkout issue2
Switched to branch 'issue2'
$ git branch
* issue2
  issue3
  master
```

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_6_1.png)

在issue2分支的myfile.txt添加commit命令的说明后提交。

```
学习Git
add 把变更录入到索引中
commit 记录索引的状态
```

```
$ git add myfile.txt
$ git commit -m "添加commit的说明"
[issue2 8f7aa27] 添加commit的说明
 1 files changed, 2 insertions(+), 0 deletions(-)
```

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_6_2.png)

接着，切换到issue3分支。

```
$ git checkout issue3
Switched to branch 'issue3'
```

打开myfile.txt档案。由于在issue2分支添加了commit命令的说明，所以issue3分支的myfile.txt里只有add命令的说明。

添加pull命令的说明后提交。

```
学习Git
add 把变更录入到索引中
pull 取得远端数据库的内容
```

```
$ git add myfile.txt
$ git commit -m "添加pull的说明"
[issue3 e5f91ac] 添加pull的说明
 1 files changed, 2 insertions(+), 0 deletions(-)
```

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_6_3.png)

这样，添加commit的说明的操作，和添加pull的说明的操作就并行进行了。

## 解决合并的冲突

把issue2分支和issue3分支的修改合并到master。

切换master分支后，与issue2分支合并。

```
$ git checkout master
Switched to branch 'master'
$ git merge issue2
Updating b2b23c4..8f7aa27
Fast-forward
 myfile.txt |    2 ++
 1 files changed, 2 insertions(+), 0 deletions(-)
```

执行fast-forward（快进）合并。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_7_1.png)

接着合并issue3分支。

```
$ git merge issue3
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Automatic merge failed; fix conflicts and then commit the result.
```

自动合并失败。由于在同一行进行了修改，所以产生了冲突。这时myfile.txt的内容如下：

```
学习Git
add 把变更录入到索引中
<<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端数据库的内容
>>>>>>> issue3
```

在发生冲突的地方，Git生成了内容的差异。请做以下修改：

```
学习Git
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

修改冲突的部分，重新提交。

```
$ git add myfile.txt
$ git commit -m "合并issue3分支"
# On branch master
nothing to commit (working directory clean)
```

历史记录如下图所示。因为在这次合并中修改了冲突部分，所以会重新创建合并修改的提交记录。这样，master的HEAD就移动到这里了。这种合并不是fast-forward合并，而是non fast-forward合并。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_7_2.png)

## 用rebase合并

合并issue3分支的时候，使用rebase可以使提交的历史记录显得更简洁。

现在暂时取消刚才的合并。

```
$ git reset --hard HEAD~
```

![rebase前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_8_1_1.png)

切换到issue3分支后，对master执行rebase。

```
$ git checkout issue3
Switched to branch 'issue3'
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: 添加pull的说明
Using index info to reconstruct a base tree...
<stdin>:13: new blank line at EOF.
+
warning: 1 line adds whitespace errors.
Falling back to patching base and 3-way merge...
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Failed to merge in the changes.
Patch failed at 0001 添加pull的说明

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To check out the original branch and stop rebasing run "git rebase --abort".
```

和merge时的操作相同，修改在myfile.txt发生冲突的部分。

```
学习Git
add 把变更录入到索引中
<<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端数据库的内容
>>>>>>> issue3
```

rebase的时候，修改冲突后的提交不是使用commit命令，而是执行rebase命令指定 --continue选项。若要取消rebase，指定 --abort选项。

```
$ git add myfile.txt
$ git rebase --continue
Applying: 添加pull的说明
```

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_8_1.png)

这样，在master分支的issue3分支就可以fast-forward合并了。切换到master分支后执行合并。

```
$ git checkout master
Switched to branch 'master'
$ git merge issue3
Updating 8f7aa27..96a0ff0
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

myfile.txt的最终内容和merge是一样的，但是历史记录如下。

![目前的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup2_8_2.png)

## topic分支和merge分支的运用实例

我们用简单的实例来讲解topic分支和merge分支的操作方法。

例如，在开发功能的topic分支操作途中，需要修改bug。

![在开发功能的主题分支操作的途中，需要进行错误的修改](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_1.png)

这时，merge分支还是处于开发功能之前的状态。在这里新建修改错误用的主题分支，就可以从开发功能的作业独立出来，以便开始新的工作。

![新建修改用的主题分支，可以从开发功能独立出来开始操作](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_2.png)

完成bug修正的工作后，把分支导入到原本的merge分支后就可以公开了。

![导入到原本的合并分支後就可以公开](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_3.png)

回到原本的分支继续进行开发功能的操作。

![回到原本的分支继续进行开发功能的操作](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_4.png)

但是，如果要继续进行操作，你会发现需要之前修正bug时提交X的内容。有2种导入提交X的内容的方法：一种是直接merge，另一种是和rebase导入提交X的合并分支。

这里我们使用rebase合并分支的方法。

![rebase到合并分支](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_5.png)

在导入提交X的内容的状态下继续进行开发功能。

> 这样，有效地利用分支的话就可以同时进行不同的作业了。

### 操作一下

以为master分支为基础, 创建o分支, 并切换到o分支:

```
$ git checkout master
$ git checkout -b o
```

此时还没在o分支更改内容, 但是master分支的代码出现的bug,现在要紧急修复bug

回到master分支,  以master分支为基础, 创建并切换到x分支:

```
$ git checkout master
$ git checkout -b x
```

在x分支修复bug, 为myfile.txt写入内容:

```
学习Git
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
bugfix
```

加入索引去并提交到数据库:

```
$ git add myfile.txt
$ git commit -m 'bug fix'
```

修复bug之后, 切换到master分支, 合并x分支:

```
$ git checkout master
$ git merge x
```

此时再回到o分支去作业:

```
$ git checkout o
```

但是o分支也需要x分支的bug修复怎么办?可以使用rebase, 从master分支取得最新的历史记录:

```
$ git rebase master
```

> 因为master分支包含了o分支所有的历史记录, 所以没有冲突, 不用手动解决冲突再 `rebase --continue`

## 一个成功的Git分支模型

这个用例主要分为

- 主分支
- 特性分支
- release分支
- hotFix分支

分别使用4个种类的分支来进行开发的。

![Git的分支操作模组](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup1_5_6.png)

### 主分支

主分支有两种：master分支和develop分支

**master**

master分支只负责管理发布的状态。在提交时使用标签记录发布版本号。

**develop**

develop分支是针对发布的日常开发分支。刚才我们已经讲解过有合并分支的功用。

### 特性分支

特性分支就是我们在前面讲解过的topic分支的功用。

这个分支是针对新功能的开发，在bug修正的时候从develop分支分叉出来的。基本上不需要共享特性分支的操作，所以不需要远端控制。完成开发后，把分支合并回develop分支后发布。

### release分支

release分支是为release做准备的。通常会在分支名称的最前面加上release-。release前需要在这个分支进行最后的调整，而且为了下一版release开发用develop分支的上游分支。

一般的开发是在develop分支上进行的，到了可以发布的状态时再创建release分支，为release做最后的bug修正。

到了可以release的状态时，把release分支合并到master分支，并且在合并提交里添加release版本号的标签。

要导入在release分支所作的修改，也要合并回develop分支。

### hotFix分支

hotFix分支是在发布的产品需要紧急修正时，从master分支创建的分支。通常会在分支名称的最前面加上 hotfix-。

例如，在develop分支上的开发还不完整时，需要紧急修改。这个时候在develop分支创建可以发布的版本要花许多的时间，所以最好选择从master分支直接创建分支进行修改，然后合并分支。

修改时创建的hotFix分支要合并回develop分支。

## 操作一下

创建并切换到开发分支develop:

```
$ git chekcout -b develop
```

创建两个特性分支, 一个用于开发功能a的fa, 一个用于开发功能b的fb:

```
$ git branch fa
$ git branch fb
```

去fa分支开发功能a:

```
$ git checkout fa
```

修改myfile.txt并提交到数据库:

```
add 把变更录入到索引中
commit 记录所以的状态
pull 取得远端数据库的内容
bugfix
fa
```

```
$ git add myfile.txt
$ git commit -m 'fa'
```

去fb分支开发功能b:

```
$ git checkout fb
```

修改myfile.txt并提交到数据库:

```
add 把变更录入到索引中
commit 记录所以的状态
pull 取得远端数据库的内容
bugfix
fb
```

```
$ git add myfile.txt
$ git commit -m 'fb'
```

回到develop分支, 合并fa的分支:

```
$ git checkout develop
$ git merge fa
```

> fa包含了develop所有的历史记录, 所以是快进合并模式

继续合并fb的分支, 因为fa和fb修改 了同一个文件, 所以会有冲突, 解决冲突后提交:

```
$ git merge fb
```

合并之后, 调整冲突的部分, 调整冲突部分后的文件内容:

```
add 把变更录入到索引中
commit 记录所以的状态
pull 取得远端数据库的内容
bugfix
fa
fb
```

提交到数据库:

```
$ git add myfile.txt
$ git commit -m '合并fb'
```

最后分别回到fa和fb,让这两个开发分支使用rebase方式同步develop的代码:

```
$ git checkout fa
$ git rebase develop

$ git checkout fb
$ git rebase develop
```

> 当然, 如果你不想要fa和fb两个分支了, 也可以直接删掉, 重新从develop分支分叉出来新的分支也可以

## pull

我们在入门篇也讲解过，执行pull可以取得远程数据库的历史记录。接下来，我们用图来讲解数据库提交的细节。

首先确认更新的本地数据库分支没有任何的更改。

![分支没有任何修改的情况](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_1_1.png)

这时只执行fast-forward合并。图中的master是本地数据库的master分支，origin/master是远程数据库的origin的master分支。

![fast-forward合并](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_1_2.png)

如果本地数据库的master分支有新的历史记录，就需要合并双方的修改。

![本地端数据库的master分支有新的历史记录](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_1_3.png)

执行pull就可以进行合并。这时，如果没有冲突的修改，就会自动创建合并提交。如果发生冲突的话，要先解决冲突，再手动提交。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_1_4.png)

## fetch

执行pull，远程数据库的内容就会自动合并。但是，有时只是想确认本地数据库的内容而不想合并。这种情况下，请使用fetch。

执行fetch就可以取得远程数据库的最新历史记录。取得的提交会导入到没有名字的分支，这个分支可以从名为FETCH_HEAD的退出。

例如，在本地数据库和远程数据库的origin，如果在从B进行提交的状态下执行fetch，就会形成如下图所示的历史记录。

![在本地端数据库和远端数据库的origin，在从B进行提交的状态下执行fetch](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_2_1.png)

在这个状态下，若要把远程数据库的内容合并到本地数据库，可以合并FETCH_HEAD，或者重新执行pull。

![合并FETCH_HEAD](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_2_2.png)

> 合并后，历史记录会和pull相同。实际上pull的内容是fetch + merge组成的。

## push

从本地数据库push到远程数据库时，要fast-forward合并push的分支。如果发生冲突，push会被拒绝的。

若要共享在本地数据库创建的分支，需要明确的push。因此，没有执行push就不会给远程数据库带来影响，因而可以自由的创建自己的分支。

![](https://backlog.com/git-tutorial/cn/img/post/stepup/capture_stepup3_3_1.png)

> 注意: 基本上，远程数据库共享的提交是不能修改的。如果修改的话，跟远程数据库同步的其他数据库的历史记录会变得很奇怪的。

### 综合实例


首先检查是否在master分支, 如果不是, 请先回到master分支:

```
$ git checkout master
```

在远程数据库平台创建一个名为lmonky的的数据库

为本地数据库设置远程数据库地址, 设置别名为`origin`:

```
$ git remote add origin https://gitee.com/liuwantao/lmonkey.git
```

将本地数据库推送到远程数据库, 第一次推送需要加上 `-u`, 并指定远程数据库地址为`origin`, 默认分支为`master`:

```
$ git push -u origin master
```

离开当前本地数据库lmonkey目录, 克隆远程数据库并命名为lmonkey2:

```
$ git clone https://gitee.com/liuwantao/lmonkey.git lmonkey2
```

在lmonkey2中, 增加一行内容, 并提交到数据库, 同时推送到远程数据库:

```
$ cd lmonkey2
$ echo 'lmonkey2' >> myfile.txt
$ git add myfile.txt
$ git commit -m 'lmonkey2'
$ git push
```

回到lmonkey目录, 在lmonkey中, 增加一行内容, 并提交到数据库, 同时推送到远程数据库:

```
$ cd lmonkey
$ echo 'lmonkey' >> myfile.txt
$ git add myfile.txt
$ git commit -m 'lmonkey'
$ git push
```

拒绝推送, 因为本地数据库并不包含远程数据库所有的历史记录, 不能自动合并, 所以拒绝推送, 必须先从远程数据库pull下来, 在本地合并之后, 再次推送

执行pull:

```
$ git pull
```

可以看到, 并没有进行自动合并, 而是冲突了, 这是因为远程数据库的历史记录, 并没有包含本地所有的历史记录, 需要解决冲突后, 再次提交

解决冲突:

```
lmonkey2
lmonkey
```

再次提交到本地数据库, 并推送到远程数据库:

```
$ git add myfile.txt
$ git commit -m '解决冲突'
$ git push
```

push成功之后, 远程数据库记录的历史记录就更新了

回到lmonkey2目录, 使用fetch查看远程数据库的更新:

```
$ cd lmonkey2
$ git fetch
```

fetch之后,远程数据库的历史记录有更新的话,会被放到FETCH_HEAD

最后使用merge就可以合并远程数据库的历史记录了:

```
$ git merge origin master
```