
cd + 目录名  把command line 切换到当前文件夹，前提是该文件夹在命令行显示的目录，

在mac终端打开应该是~

git init 因为已经切换到当前文件夹，你现在输入的指令就是针对当前文件夹的，该指令指把该
目录变成Git可以管理的仓库。

---

## 一、把文件放在该目录后用指令：git add file

意思是把该文件从工作区添加到git托管，放进stage，或者叫index，暂存区，注意此时该文件并没有放到
git的master主分支。

该命令可以这么操作：

git add file1 file2 file3

可以一次性提交多个文件到暂存区。

## 二、用git commit -m"xxx"提交更改，实际上就是把暂存区的所有内容提交到当前的主分支。

说明-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便
地找到改动记录，养成一个好习惯，写清楚提交说明，方便你管理。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往
master分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

## git status命令可以让我们时刻掌握仓库当前的状态

虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容岂不美滋滋，但是你如果忘了
怎么修改的，就可以用这个命令：

## git diff file

顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从命令输出看到

## 版本退回：

一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作。

在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。

git这个commit的概念，就是版本号。怎么查看呢？

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看，这个命令是查看我们提交的历史，就是commit的历史

如果觉得git log这个命令给的信息太多，可以加上一点儿指令简化log信息

`git log --pretty=oneline`

这时候你得到的就是精简的一行的信息

你看到的那一大串的数字就是所谓commit id版本号

## 有两种方法往前退

### 第一种方法往“未来”去。

有两种方法往前退有一种方法往“未来”去。

在这之前先解决另一个概念因为Git在内部有个指向当前版本的HEAD指针，它来控制指向的版本。

这时候第一种往前退的方法就是用这个符号^，用这个指令

`git reset --hard HEAD^`

解释一下，HEAD^的意思就是退回上一个版本，聪明如你可能明白了，退回上上个版本用HEAD^^，退回
上上上个版本就是.........以此类推，版本号呢？

第二种方法就是用版本号了，`git reset --hard commit_id`

怎么得到commit_id的方法还记得吗？

`git log`

然后用上面的指令跳回版本号所代表的那个版本，可以隔着跳。

### 最后一种，就是往“未来”跳
什么意思呢？假如当前版本为0号，前一个版本为-1号，在前一个版本是-2号。

这时候我们跳到了-2这个版本，我们想往0那儿跳怎么做呢？

你也许会说用commit id啊，对，是用commit id，但怎么用呢？

git log？你试试就知道，上面我提到过，这个命令是提交历史，因为这时候你的HEAD指针指向的是-2的历史，
意思就是现在仓库以为你当前提交的就是-2，它会以为你最新的就是-2，
所以git log就只会显示到-2这个版本的历史，而-1和0都没有出现，这时候就要用另一个指令：

`git relog`

这个指令跟git log的区别在于，这个指令记录的是指令的历史，就是你对这个仓库所发出的指令的历史。
它会包含你从0跳到-2的过程中发出的指令。所以在这个指令中查找0的commit id从而用：

`git reset --hard commit_id`来实现跳回未来吧。

在版本退回的过程中你可能会用到这个指令：

`cat file`

可以看到文件的内容。


场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令
`git reset HEAD file`，用命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重
新放回工作区就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本退回的讲解，不过前提是没有推送
到远程库。

## 删除文件：

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交：

```
$ git add test.txt
$ git commit -m "add test.txt"
[master 94cdc44] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

 一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
 
`$ rm test.txt`


这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
```
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       deleted:    test.txt
#
no changes added to commit (use "git add" and/or "git commit -a")

```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
```
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
[master d17efd8] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

 现在，文件就从版本库中被删除了。

 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

`$ git checkout -- test.txt`

 git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。


## 添加到远程库：
如github
你要先设置一下SSH，可以自行百度。比较简单。

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git；`

server-name：path，就用github.com:username，

username用你的github账号用户名，

repo-name用你在github上已经创建的仓库的名字，就是把你的本地仓库托管到github的你建立的仓库，让他们关联。

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；
就是推送你本地仓库的所有内容，不管你这个本地仓库的文件夹有多少文件。

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

## 从远程仓库克隆：
既然可以把本地的东西同github上的你建立的仓库关联，我们可以去github这个远程仓库中拷贝我们本地没有的仓库吗？

当然可以，比如用这个指令`git clone git@github.com:wnz27/gitskills.git`

这个指令的意思是把wnz27这个用户名的主页建立的gitskills这个仓库拷贝到本地电脑上。

所以你们需要把用户名替换掉还有仓库名（gitskills）替换掉。

GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地
址。

实际上，Git支持多种协议，默认的`git://使用ssh，但也可以使用https等其他协议。`

什么意思呢？就是这个指令可以这样输：

`git clone https://github.com/wnz27/gitskills.git`

使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的
公司内部就无法使用ssh协议而只能用https。
还记得刚才让你们自行百度SSH吗？如果你们不设置，就只能用https的协议了,只能用后一个指令。

Git支持多种协议，包括https，但通过*ssh支持的原生git协议速度最快*。


## 问题：
在使用git 对源代码进行push到gitHub时可能会出错，信息如下：

```
To github.com:wnz27/27.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:wnz27/27.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

出现错误的主要原因是github中的README.md文件不在本地代码目录中
可以通过如下命令进行代码合并

`git pull --rebase origin master`

就是把你github仓库里的readme的文件拉到本地来放进本地的目录里。

此时再执行语句
`git push -u origin master`即可完成代码上传到github

