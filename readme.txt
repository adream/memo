P:\app\GitHub> mkdir memo
 
 
    目录: P:\app\GitHub
 
 
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2016-11-21     10:47            memo
 
 
P:\app\GitHub> dir
 
 
    目录: P:\app\GitHub
 
 
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2016-11-17     14:22            https---github.com-taobao-nginx-bo
                                             ok.git
d----        2016-11-21     10:47            memo
d----        2016-11-18     15:19            nginx-book
d----        2016-11-21     10:14            run-jekyll-on-windows
 
 
P:\app\GitHub> cd memo
P:\app\GitHub\memo> git init
Initialized empty Git repository in P:/app/GitHub/memo/.git/
P:\app\GitHub\memo [master]> ls -ah
 
 
    目录: P:\app\GitHub\memo
 
 
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d--h-        2016-11-21     10:51            .git




P:\app\GitHub\memo [master +1 ~0 -0 ~]> git commit -m "wrote a readme file"
[master (root-commit) bcc38f1] wrote a readme file
1 file changed, 2 insertions(+)
create mode 100644 readme.txt
P:\app\GitHub\memo [master]>

////
初始化一个Git仓库，使用git init命令。
 
添加文件到Git仓库，分两步：
 
第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
 
第二步，使用命令git commit，完成。
////
要随时掌握工作区的状态，使用git status命令。
 
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
////
 git log --pretty=oneline
P:\app\GitHub\memo [master]> git log --pretty=oneline
7d081a27d2c4902e1d2f4b7b0f7d027c69505f5a append GPL
14d560c91c83f9d0c607dcee0dff580d469b4aa4 add two lines
bcc38f16b78591413cd6e24ea71227f90e291340 wrote a readme file

Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交的，上一个版本是HEAD^，上上一个版本是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

回退到上一个版本可以使用git reset命令：
$ git reset --hard HEAD^
git reset --hard HEAD~1
P:\app\GitHub\memo [master]> git reset --hard HEAD 7d081
fatal: Cannot do hard reset with paths.
P:\app\GitHub\memo [master]> git reset  HEAD 7d081

////
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

////
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
////原理
工作区（Working Directory）就是你在电脑里能看到的目录，比如我的memo文件夹就是一个工作区：
版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
git-repo

////
可以将working dir，stage，master 看成版本的三个状态，通过git status可以看出三个状态的区别：
 
"changes not staged for commmit"//working dir和stage不同（working dir 做了修改）
"changes to be committed"//stage和master有区别（新的修改被add到stage）
上面两种情况同时出现，三个状态都不相同（可能对应为：working dir 作了修改后d但未git commit，然后对working dir 又做了修改）
"working tree clean" 三个状态保持一致
所以
 
git checkout保持working dir 与stage一致，以stage为主
git reset保持stage与master一致，以master为主
在git status可以看到
 
git checkout提示信息时出现在changes not staged for commit下的作用是"discard changes in working dir"，对应的是working dir和stage的关系
git reset提示信息出现在changes to be committed下作用是"unstage"对应的是stage和master之间的关系
////
git remote add origin https://github.com/adream/memo.git


echo "# memo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/adream/memo.git
git push -u origin master


 
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

git clone git@github.com:adream/learnit.git

 GitHub给出的地址不止一个，还可以用https://github.com/adream/learnit.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000