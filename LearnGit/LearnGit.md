<font color=blue>主要内容和资料来着廖雪峰的官方网站的Git教程
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000</font>
&nbsp;
&nbsp;
&nbsp;


# 安装 

    `git config --global user.name "Your Name"`  
    `git config --global user.email "email@example.com"`  
    `git config --global color.ui true` # [opitional] 让Git显示颜色，会让命令输出看起来更醒目  
&nbsp;
    
    以下是一些optional的alias  
    `git config --global alias.st status`   
    `git config --global alias.co checkout`  
    `git config --global alias.ci commit`  
    `git config --global alias.br branch`  
    `git config --global alias.unstage 'reset HEAD'`  
    `git config --global alias.last 'log -1'`  
    `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'--abbrev-commit"`

    * 配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
    * 每个仓库的Git配置文件都放在`.git/config`文件中
    * 当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中


# 初始化一个Git仓库  
  
    `mkdir learngit`  
    `cd learngit`  
    `git init`


# 工作区、版本库、暂存区  

## 工作区（Working Directory）  
  电脑里能看到的目录，比如这里示例的learngit文件夹  

## 版本库 (Repository)  
* 工作区有一个隐藏目录.git
  * Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
  
  * `git add` 实际上就是把文件修改添加到暂存区
  * `git commit`实际上就是把暂存区的所有内容提交到当前分支
  * 如果不add到暂存区，那就不会加入到commit中


add ![title](http://leanote.com/api/file/getImage?fileId=56eb9974ab644175ec003739)

commit ![title](http://leanote.com/api/file/getImage?fileId=56eb995bab644175ec003734)
&nbsp;
&nbsp;

# 添加文件到Git仓库，分两步 (add 和 commit）  
* `git add README1.txt`  

* `git add README2.txt TEST.py`  
* `git commit -m  "add 3 files"`  
* commit 可以一次提交很多文件，所以你可以多次 add 不同的文件  


# 查看工作区的状态  
* git status告诉你有没有文件被修改过  
    >`git status `  
    
    >On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   readme.txt
no changes added to commit (use "git add" and/or "git commit -a")
&nbsp;

* git diff可以查看修改内容     
    > `git diff README1.txt`   
    
    >diff --git a/readme.txt b/readme.txt
index e69de29..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -0,0 +1,2 @@
+Git is a distributed version control system.
+Git is free software.


# 删除  
* 从本地删除  
  `rm test.txt`  
* 从版本库中删除该文件，那就用命令git rm 删掉，并且git commit  
`git rm test.txt`  
`git commit -m "remove test.txt"`  

* 命令`git rm` 用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。  

# 历史记录  
* `git log` 命令显示从最近到最远的提交日志  
  * `git log`  
  * `git log --pretty=oneline`  
  * 和SVN不一样，Git的commit id（版本号）不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示
&nbsp;

* git reflog用来记录你的每一次命令
    不用担心版本退回后commit id 找不到了
    `git reflog` 


# 回退到以前的版本  
* Git在内部有个指向当前版本的HEAD指针  

* 上一个版本就是HEAD\^，上上一个版本就是HEAD\^\^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100  
  * `git reset --hard HEAD^`  ## 退回到上一个版本  
  * `git reset --hard 3628164`  ## 退回到某个commit id  
    * 版本号没必要写全，前几位就可以了，Git会自动去找  

* 回退到以前的版本后，用`git log `就再看不见“以前的以后”的commit id了，这时候用`git reflog`还可以查看到



# 撤销修改/恢复删除  
* `git checkout -- readme.txt`可以丢弃工作区的修改  
  * 如果readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
  * 如果readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
  * 总之，就是让这个文件回到最近一次`git commit`或`git add` 时的状态。

* `git reset HEAD readme.txt` 可以把暂存区的修改撤销掉（unstage），重新放回工作区
&nbsp;

    ![title](http://leanote.com/api/file/getImage?fileId=56eb990aab6441777b0039ad)
* 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
* 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
* 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。


# 远程仓库
##  GitHub
* 注册GitHub账号
* 创建SSH Key  
在用户主目录下，看看有没有`.ssh`目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果没有: 
`ssh-keygen -t rsa -C "youremail@example.com"`
一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码

* 登陆GitHub，打开“Account settings”，“SSH Keys”页面： 然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

* 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

* [GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)
&nbsp;  

## 添加远程库
* 登陆GitHub，然后在右上角找到“Create a new repo”按钮，创建一个新的仓库;  
在Repository name填入`repo-name`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库。
* 在本地的learngit仓库下运行命令：  
    `git remote add origin git@github.com:username/repo-name.git`  
    添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

* 把本地库的所有内容推送到远程库  
    `git push -u origin master` # 把当前分支master推送到远程  
    
    由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
* 以后只要本地作了提交，就可以通过命令 `git push origin master` 把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库
&nbsp;

## 从远程库克隆
* `git clone git@github.com:username/repo-name2.git`
* Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议  
    使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https
&nbsp;

# 分支管理
* 创建分支  
    `git branch dev`   # 创建  
    `git checkout dev` # 切换  
    `git checkout -b dev`  # 加上`-b`参数后创建同时切换  
    
*  查看当前分支  
    `git branch`命令会列出所有分支，当前分支前面会标一个*号
* 切换回`master`分支
     `git checkout master`
* 合并指定分支到当前分支  
    `git merge dev`  
    这里`Fast-forward`(快进模式)，是直接把master指向dev的当前提交
* 删除分支  
    `git branch -d dev`
&nbsp;
![title](http://leanote.com/api/file/getImage?fileId=56ebb797ab6441777b003bd1)
    <font color=blue>因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。</font>
&nbsp;
* 分支策略
  * 合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息  
  (记录里看不出来曾经做过合并)
  
  * 禁用快速模式  
 `git merge --no-ff -m "merge with no-ff" dev`  
        
        merge结果如下 (dev的分支记录被保存下来了)
    ![title](http://leanote.com/api/file/getImage?fileId=56ebb6e9ab644175ec0039c6)
  * git  
  `git log --graph --pretty=oneline --abbrev-commit`

  > \*   7825a50 merge with no-ff  
    |\  
    | \* 6224937 add merge  
    |/  
    \*   59bc1cb conflict fixed  
    ...  
&nbsp;

* 团队合作分支策略
  * `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活
  
  * 干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本
  * 每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了
    ![title](http://leanote.com/api/file/getImage?fileId=56ebb908ab6441777b003bed)
&nbsp;

# 分支冲突

* `master`分支和`feature1`分支各自都分别有新的提交  
![title](http://leanote.com/api/file/getImage?fileId=56ebb4b1ab644175ec0039aa)

* Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突  
    >`git merge feature1`  
    
Auto-merging readme.txt  
CONFLICT (content): Merge conflict in readme.txt  
Automatic merge failed; fix conflicts and then commit the result  

* 查看文件冲突的部分  
* 手动修改  
* 提交修改后的文件  
`git add readme.txt `  
`git commit -m "conflict fixed"`  
* 查看分支的合并情况  
>`git log --graph --pretty=oneline --abbrev-commit`  

  >\*   59bc1cb conflict fixed  
|\  
| \* 75a857c AND simple  
\* | 400b400 & simple  
|/  
\* fec145a branch test  
...
 ![title](http://leanote.com/api/file/getImage?fileId=56ebb414ab6441777b003bb3)
&nbsp;

* 删除feature1分支  
`git branch -d feature1`

* (另) 丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除  


#  Bug分支
* 不提交，把当前工作现场“储藏”起来  
`git stash`  
>Saved working directory and index state WIP on dev: 6224937 add merge  
HEAD is now at 6224937 add merge  
* 回来  
`git checkout dev`  
`git stash list`  
>stash@{0}: WIP on dev: 6224937 add merge  
* 恢复
  * 用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除
     ` git stash apply stash@{0}` 恢复指定的stash
  
  * 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了
&nbsp;

# 标签管理
* Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像，但是分支可以移动，标签不能移动）  
* 切换到需要打标签的分支， 用`git tag <name>`就可以打一个新标签。  
` git tag v1.0`  
  
  默认标签是打在最新提交的commit上的  
* 用`git tag <name> <commit id>`给历史上的某次commit打标签  
* 创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字  
`git tag -a v0.1 -m "version 0.1 released" 3628164`  
* 通过-s用私钥签名一个标签  
`git tag -s v0.2 -m "signed version 0.2 released" fec145a`  

  签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错; 如果报错，请参考GnuPG帮助文档配置Key。  
* 用命令`git tag` 查看所有标签  
* 用`git show <tagname>`查看标签信息  

* 推送  
`git push origin <tagname>`  
`git push origin --tags`  # 一次性推送全部尚未推送到远程的本地标签  
* 删除未推送的本地标签  
`git tag -d <tagname>`  
* 删除远程标签  
`git tag -d <tagname>` # 先删除本地  
`git push origin :refs/tags/<tagname>` # 再删除远程  























