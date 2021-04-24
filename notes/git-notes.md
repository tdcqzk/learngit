## git使用(windows)

### 1.安装和设置

#### (1)直接安装

#### (2)名字和邮箱

``` bash
$ git config --global user.name "Your Name" #全局
$ git config --global user.email "email@example.com"
```

### 2.创建版本库并且增删改查

#### (1)基本命令

```bash
mkdir cd pwd 
git inint
```

#### (2)提交文件到仓库

```bash
git add readme.txt
git commit -m "add a file to repo"
```

#### (3)修改

```bash
#改完后看状态，差异以及提交
git status 
git diff xx.txt
git add xx.txt
git commit -m "add two stentences into readme.txt"
```

#### (4)回退或者改变版本

```bash
git log #查看日志
git log --pretty=oneline#查看日志,显示在一行，整洁
git reset --hard HEAD^ #上一个，HEAD^^上两个，HEAD~100,100个
#又想回到新版本？
git reflog # 查看操作日志，找到从哪里跳过来的
git reset --hard (command id前几位)
```

#### (5) working directory and repo

*  repo  add-->  缓存区  commit-->工作区

* 每次修改都要用add 否则是没有进入缓存区的，改了两次，只有第一次add，这时候commit只能将在缓存区的放进工作区

* 用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

  ```bash
  git diff HEAD -- readme.txt
  ```

* 关于回退：

  * 在工作区做了修改，没add用

    ```bash
    git checkout -- readme.txt
    ```

  * 已经add到缓存区

    ```bash
    git reset HEAD readme.txt
    #then
    git checkout -- readme.txt
    ```

  * 已经传到repo了

    ```bash
    git reset --hard HEAD^ #这样刚刚做的修改,add以及commit全都无了，谨慎操做（虽然可以回退）
    ```

* 上面用到的checkout在切换库的时候用到，因此此处用 -- 做参数加以区分，新版本git引入restore,可以代替；

  * 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令

    ```bash
    git checkout -- file 
    #或者 
    git restore readme.txt 
    ```

    

  * 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令

    ``` bash
    git reset HEAD <file>
    #或者
    git restore --staged readme.txt 
    #就回到了场景1，第二步按场景1操作。
    ```

    

  * 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交

    ```bash
    git reset --hard HEAD^ #这样刚刚做的修改,add以及commit全都无了，谨慎操做（虽然可以回退）
    ```

#### (6)删除和恢复

* 删除

  ```bash
  rm test.txt #工作区删除
  git rm test.txt #then
  git commit -m "删除了test.txt"
  ```


* 恢复

  ```bash
  #在repo删除了
  git restore test.txt
  #在工作区删除了，无法恢复
  ```

  

### 3.github

#### (1)在github建立仓库

#### (2)本地生成公钥

```bash
ssh-keygen -t rsa -C "2319750740@qq.com"
#填入存储路径，一般win10在c/Users/Dell/.ssh/rsa_id下面
#完成之后
cd ~/.ssh #直接到达目录
ls #查看是否有id_rsa 和 id_rsa.pub,然后
cat id_rsa.pub #把他复制下来，连同前面的rsa和后面的邮箱信息
```

#### (3)把公钥弄到github上

* github 头像 ssh and GPG keys，打开新建keys粘贴进去就好了

#### (4)开始链接

```bash
git remote add origin git@github.com:tdcqzk/learngit.git
git push -u origin master 
#之后再要推送时，直接
git push origin master
#如果自己在github做了修改，再推可能出问题，这是可以强制推上去
git push -f origin master
```

#### (5)删除远程库

```bash
#先看看有哪些远程库
git remote -v
#挑出你要删除的，比如之前输错的ori
git remote rm ori
```



## ps:帮助文档

```bash
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```