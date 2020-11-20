## 为什么用Git

要把文档还原到编辑前的状态，大家都是怎么做的呢？

最简单的方法就是先备份编辑前的文档。使用这个方法时，我们通常都会在备份的文档名或目录名上添加编辑的日期。但是，每次编辑文档都要事先复制，这样非常麻烦，也很容易出错。

![备份文档的实例](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_1_1.png)

再加上，如果像上图那样毫无命名规则的话，就无法区分哪一个文档是最新的了。而且，如果是共享文件的话，应该加上编辑者的名字。还有，那些文档名字没有体现修改内容。

另外，如果两个人同时编辑某个共享文件，先进行编辑的人所做的修改内容会被覆盖，相信大家都有这样的经历。

![共享文件操作失败实例](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_1_2.png)

Git版本管理系统就是为了解决这些问题应运而生的。

### 使用Git进行版本管理

Git是一个分布式版本管理系统，是为了更好地管理Linux内核开发而创立的。

Git可以在任何时间点，把文档的状态作为更新记录保存起来。因此可以把编辑过的文档复原到以前的状态，也可以显示编辑前后的内容差异。

而且，编辑旧文件后，试图覆盖较新的文件的时候（即上传文件到服务器时），系统会发出警告，因此可以避免在无意中覆盖了他人的编辑内容。

![使用版本管理的共享文件操作实例](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_1_3.png)

```
用Git管理文件的话，更新的历史会保存在Git，所以，不需要备份文件，非常的方便。
```



## 安装Git

首先下载你的系统的Git客户端

[Git下载地址](https://git-scm.com/downloads)

这里以Windows举例, 下载之后, 双击安装即可

安装完成之后请执行 version命令，如果显示Git的版本就说明安装成功了。

```
git --version
```

## 管理历史记录的仓库

仓库 (Repository)是记录文件或目录状态的地方，存储着内容修改的历史记录。在仓库的管理下，把文件和目录修改的历史记录放在对应的目录下。

![管理文件或目录的历史记录的数据库](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_2_1.png)

### 远程仓库和本地仓库

首先，Git的仓库分为远程仓库和本地仓库的两种。

**远程仓库:**

配有专用的服务器，为了多人共享而建立的仓库。

**本地仓库:**

为了方便用户个人使用，在自己的机器上配置的仓库。

仓库分为远程和本地两种。平时用手头上的机器在本地仓库上操作就可以了。如果想要公开在本地仓库中修改的内容，把内容上传到远程仓库就可以了。另外，通过远程仓库还可以取得其他人修改的内容。

![远程数据库和本地数据库](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_2_2.png)

## 创建仓库

创建本地仓库的方法有两种：一种是创建全新的仓库，另一种是复制远程仓库。

接下来要在本地新建仓库，创建一个名称为「demo」的空目录，并把它放在Git管理之下。

下面将以这个目录进行教程讲解。

首先在任意一个地方创建demo目录。然后使用init命令把该demo目录移动到本地Git仓库。

按照以下步骤把新创建的demo目录设置到Git仓库

桌面空白处右键打开 Git Bash, 执行命令：(git命令与linux命令基本相同)

```
$ mkdir demo
$ cd demo
$ git init
```

## 工作树和索引

在Git管理下，大家实际操作的目录被称为工作树。

在仓库和工作树之间有索引，索引是为了向仓库提交作准备的区域。

![工作树和索引](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro1_4_1.png)

Git在执行提交的时候，不是直接将工作树的状态保存到仓库，而是将设置在中间索引区域的状态保存到仓库。因此，要提交文件，首先需要把文件加入到索引区域中。

所以，凭借中间的索引，可以避免工作树中不必要的文件提交，还可以将文件修改内容的一部分加入索引区域并提交。

## 一些开始的设定

安装Git之后，请输入您的用户名和电子邮件地址。该设置操作在安装Git后进行一次就够了。这些信息将作为提交者信息显示在更新历史中。

Git的设定被存放在当前登陆用户目录的.gitconfig档案里。虽然可以直接编辑配置文件，但在这个教程里我们使用config命令。

>受国内网络影响, `Github` 访问并不是很理想, 这里使用国内的远程git仓库 ，注册 [码云](https://gitee.com/) 仓库账号
>

```
$ git config --global user.name "你的用户名"
$ git config --global user.email "你的电子邮件"
```

## 提交文件

在demo目录新建一个文件，然后将文件添加到仓库。

首先在demo目录里新建一个名为「a.txt」的文本文件，请在文件中输入以下的内容：

```
学习Git
```

请使用status命令确认工作树和索引的状态。

```
$ git status
```

执行status命令以确认demo目录的状态。

```
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#     a.txt
nothing added to commit but untracked files present (use "git add" to track)
```

从`status`响应我们可以看到`a.txt`目前不是历史记录对象。请首先把`a.txt`这个文件加入到索引，就可以追踪它的变更了。

现在，我们把`a.txt`加入到索引然后确认一下。

```
$ git add a.txt
查看状态
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#     new file:   a.txt
#
```

>   将文件加入到索引，要使用add命令。在<file>指定加入索引的文件。用空格分割可以指定多个文件。

```
$ git add <file>..
```

>   指定参数「./」，可以把所有的文件加入到索引。

```
$ git add ./
```



既然`a.txt`已加入到索引，我们就可以提交文件了。

请执行如下显示的commit命令(为之前添加的`a.txt`添加上传描述)。

```
$ git commit -m "first commint"
```

> `-m` 参数为提交的说明, 建议不为空

执行commit命令之后确认状态。

```
# 刚才添加的命令
$ git commit -m "first commit"
[master (root-commit) 116a286] first commit
 0 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt

# 查看状态
$ git status
# On branch master
nothing to commit (working directory clean)
```

从status响应我们可以看到没有新的变更要提交。

>   使用log命令，我们可以在仓库的提交记录看到新的提交。

```
$ git log
```

## push到远程仓库

前面我们为大家介绍了本地仓库的基本使用方法。下面，我们接着为大家讲解如何在远程仓库上共享本地仓库的修改记录。

### 推送

为了将本地仓库的修改记录共享到远程仓库，必须上传本地仓库中存储的修改记录。

为此，需要在Git执行推送(Push)操作。执行Push之后，本地的修改记录会被上传到远程仓库。所以远程仓库的修改记录就会和本地仓库的修改记录保持同步。

## 建立远程仓库

受国内网络影响, `Github` 访问并不是很理想, 这里使用国内的远程仓库

注册 [码云](https://gitee.com/) 账号, 并创建一个仓库, 名字为 demo:

![](http://cdn.wkbook.cn/img/20201118011901.png)

输入名字后，可以直接提交。

![](http://cdn.wkbook.cn/img/20201118012117.png)

添加成功之后, 这里可以看远程仓库的地址:

![](http://cdn.wkbook.cn/img/20201118012310.png)

## 推送到远程仓库

我们试试推送在之前创建的本地仓库吧。

向远程仓库推送本地仓库的修改记录吧。

您可以给远程仓库取一个别名。这样，下次推送的时候就不需要输入长串的远程仓库地址了。在这个教程里，我们的远程仓库命名为“origin”。

请使用remote指令添加远程仓库。输入远程仓库名称，指定远程仓库的URL。

```
$ git remote add 你的仓库名 你的仓库url地址
```

通过运行以下指令，将创建于上一个页面的远程仓库的URL命名为“origin”。

```
$ git remote add origin https://gitee.com/12356/demo.git
```

> url 可以在远程仓库的控制面板获取

> 执行推送或者拉取的时候，如果省略了远程仓库的名称，则默认使用名为”origin“的远程仓库。因此一般都会把远程仓库命名为origin。

使用push命令向仓库推送更改内容。

输入目标地址，指定推送的分支（我们将在高级篇详细地对分支进行说明。）：

```
$ git push <库名> <分支>...
```

运行以下命令便可向远程仓库‘origin’进行推送。当执行命令时，如果您指定了-u选项，那么下一次推送时就可以省略分支名称了。但是，首次运行指令向空的远程仓库推送时，必须指定远程仓库名称和分支名称。

当被要求输入用户名和密码，请使用您的码云的用户名和密码。

```
$ git push -u origin master
Username: <用户名>
Password: <密码>
Counting objects: 3, done.
Writing objects: 100% (3/3), 245 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://gitee.com/123123/demo.git
 * [new branch]      master -> master
```

> 如果使用公钥认证, 就不用再输入密码了, 高级篇会讲到

请打开码云的Git页面。刷新页面就可以看到刚刚推送到远程仓库的项目。

## 为什么要克隆

如果远程仓库中有他人的修改记录，那么把它完整地复制下来您就可以接着进行工作了。

或者您要在别人的基础上, 继续开发项目, 都可以从远程仓库克隆一份代码到本地

进行克隆（Clone）操作就可以复制远程仓库。

执行克隆后，远程仓库的全部内容都会被下载。之后您在另一台机器的本地仓库上进行操作。

> 克隆后的本地仓库的变更履历也会被复制，所以可以像原始的仓库一样进行查看记录或其他操作。

## 开始克隆

假设您是其中一位团队成员，把现有的远程仓库克隆到另一个目录( demo2 )。

使用clone指令可以复制仓库，在<repository>指定远程仓库的URL，
在<directory>指定新目录的名称。

```
$ git clone <repository> <directory>
```

执行以下指令后，会在目录(demo2) 复制远程仓库。

```
$ git clone https://gitee.com/123123/demo.git demo2
Cloning into 'demo2'...
Username: <用户名>
Password: <密码>
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
```

若要验证克隆是否成功，请看在复制的目录“demo2”中的a.txt是否含有以下文字。

```
学习Git
```

## 从克隆的仓库进行push

>   cd到demo2进行的操作

首先，在克隆的仓库目录里的`a.txt`文件里添加一行文字（方便我们辨认），并保存。

```
学习Git
add 把变更录入到索引中
```

添加到索引区, 并提交到本地仓库:

```
$ git add a.txt
$ git commit -m "添加add的说明"
[master 1ef5c8c] 添加add的说明
 1 files changed, 1 insertions(+), 1 deletions(-)
```

然后，推送此次变更，更新远程仓库。

当在克隆的仓库目录执行推送时，您可以省略仓库和分支名称。

```
$ git push
Username: <用户名>
Password: <密码>
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 351 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://gitee.com/liuwantao/demo.git
   486789c..1ef5c8c  master -> master
```

访问码云 `demo` 仓库的首页, 刷新页面就可以看到推送的更新

## 从远程仓库pull

若是共享的远程仓库由多人同时作业，那么作业完毕后所有人都要把修改的内容推送到远程仓库。然后，自己的本地仓库也需要更新其他人推送的变更内容。

## pull

进行拉取(Pull) 操作就可以把远程仓库的内容更新到本地仓库。

进行拉取(Pull) 操作，就是从远程仓库下载最近的变更日志，并覆盖自己本地仓库的相关内容。

我们把在上一节中从“demo2”推送到远程仓库的内容拉取到仓库目录“demo”吧。

使用pull指令进行拉取操作。省略仓库名称的话，会在名为origin的仓库进行pull。



命令格式：

```
$ git pull <库名> <分支名>...
```

**用demo进行的操作**

cd到一开始的demo目录，执行以下指令：

```
$ git pull origin master
Username: <用户名>
Password: <密码>
From https://gitee.com/liuwantao/demo.git
 * branch            master     -> FETCH_HEAD
Updating ac56e47..3da09c1
Fast-forward
 a.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

`a.txt`文档的内容已更新。

## 合并修改记录


![](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro5_1_1.png)

在执行pull之后，进行下一次push之前，如果其他人进行了推送内容到远程仓库的话，那么你的push将被拒绝。


![](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro5_1_2.png)

这种情况下，在读取别人push的变更并进行合并操作之前，你的push都将被拒绝。这是因为，如果不进行合并就试图覆盖已有的变更记录的话，其他人push的变更（图中的提交C）就会丢失。

> 合并的时候，Git会自动合并已有的变更点！不过，也存在不能自动合并的情况。在下一节页面，我们会为大家介绍手动合并的方法！这一节, 先看一下冲突的情况

## push冲突的状态

现在，我们将要学习怎样解决冲突。
首先，我们用“demo”和“demo2”制造一个冲突状态。

**用demo2进行的操作**

接下来，打开demo2目录的a.txt文档，添加一行文字之后进行提交。

```
学习Git
add 把变更录入到索引中
pull 取得远端仓库的内容
```

```
$ git add a.txt
$ git commit -m "添加pull的说明"
[master 4c01823] 添加pull的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
```

 现在从demo2 推送内容到远程仓库。

 ```
 $ git push
Username: <用户名>
Password: <密码>
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 391 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://gitee.com/liuwantao/demo.git
   3da09c1..4c01823  master -> master
 ```

 > 在目前的远程仓库，"a.txt"文档已包含第三行内容「pull 取得远端仓库的内容」，并且已被存储到历史记录中啦。

 **用demo进行的操作**

首先，打开demo目录的a.txt文档，添加一行文字之后进行提交。

```
学习Git
add 把变更录入到索引中
commit 记录索引的状态
```

```
$ git add a.txt
$ git commit -m "添加commit的说明"
[master 95f15c9] 添加commit的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
```

 现在从demo推送内容到远程仓库吧。

 ```
 $ git push
Username: <用户名>
Password: <密码>
To https://gitee.com/liuwantao/demo.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://gitee.com/liuwantao/demo.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
 ```

 看到吧，发生了错误，推送被拒绝（rejected）了。

 下一节, 来看一下如何解决冲突吧

 ## 解决冲突

在上一个节我们提及到，执行合并即可自动合并Git修改的部分。但是，也存在无法自动合并的情况。

如果远程仓库和本地仓库的同一个地方都发生了修改的情况下，因为无法自动判断要选用哪一个修改，所以就会发生冲突。

Git会在发生冲突的地方修改文件的内容，如下图。所以我们需要手动修正冲突。

![](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro5_1_3.png)

> ==分割线上方是本地仓库的内容,
> 下方是远程仓库的编辑内容。

如下图所示，修正所有冲突的地方之后，执行提交。

![](https://backlog.com/git-tutorial/cn/img/post/intro/capture_intro5_1_4.png)

## 解决demo与demo2的冲突

**用demo进行的操作**

`cd`在`demo`执行以下指令，让git修改我们的冲突文件 （`a.txt`）。

```
$ git pull origin master
Username: <用户名>
Password: <密码>
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://gitee.com/liuwantao/demo.git
 * branch            master     -> FETCH_HEAD
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt
Automatic merge failed; fix conflicts and then commit the result.
```

显示合并时发生冲突的讯息。

讯息显示「Merge conflict in a.txt」。我们看到Git已添加标示以显示冲突部分。请为Git无法完成主动合并的部分做以下的修改。

打开`a.txt`：

```
## 冲突情况
学习Git
add 把变更录入到索引中
<<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端仓库的内容
>>>>>>> 4c0182374230cd6eaa93b30049ef2386264fe12a
```

导入两方的修改，并删除多余的标示行以解决冲突。

```
## 修改后的
学习Git
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端仓库的内容
```

文件的内容发生了修改，所以需要进行提交。

```
$ git add a.txt
$ git commit -m "合并"
[master d845b81] 合并
```

这样就完成了从远程仓库导入最新的修改内容。

我们可以用log命令来确认仓库的历史记录是否准确。指定--graph选项，能以文本形式显示更新记录的流程图。指定--oneline选项，能在一行中显示提交的信息。

```
$ git log --graph --oneline
*   d845b81 合并
|\
| * 4c01823 添加pull的说明
* | 95f15c9 添加commit的说明
|/
* 3da09c1 添加add的说明
* ac56e47 first commit
```

这表明两个修改记录已经整合了。

这时候，之前被拒绝的push应该可以通过了，push一下看看吧。