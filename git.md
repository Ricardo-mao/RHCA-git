# git

## 1.简介

一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。

Git也是目前最流行的分布式版本控制系统，它和其他版本控制系统的主要差别在于Git只关心文件数据的整体是否发生变化，而大多数版本其他系统只关心文件内容的具体差异，这类系统（CVS，Subversion，Perforce，Bazaar 等等）每次记录有哪些文件作了更新，以及都更新了哪些行的什么内容。

### 1.1.git特性

- **分布式**：Git版本控制系统是一个分布式的系统，是用来保存工程源代码历史状态的命令行工具。
- **保存点**：Git的保存点可以追踪源码中的文件, 并能得到某一个时间点上的整个工程项目的状态；可以在该保存点将多人提交的源码合并, 也可以回退到某一个保存点上。
- **Git离线操作性**：Git可以离线进行代码提交，因此它称得上是完全的分布式处理，Git所有的操作不需要在线进行；这意味着Git的速度要比SVN等工具快得多，因为SVN等工具需要在线时才能操作，如果网络环境不好， 提交代码会变得非常缓慢。
- **Git基于快照**：SVN等老式版本控制工具是将提交点保存成补丁文件，Git提交是将提交点指向提交时的项目快照，提交的东西包含一些元数据(作者，日期，GPG等)。
- **Git的分支和合并**：分支模型是Git最显著的特点，因为这改变了开发者的开发模式，SVN等版本控制工具将每个分支都要放在不同的目录中，Git可以在同一个目录中切换不同的分支。
- **分支即时性**：创建和切换分支几乎是同时进行的，用户可以上传一部分分支，另外一部分分支可以隐藏在本地，不必将所有的分支都上传到GitHub中去。
- **分支灵活性**：用户可以随时创建、合并、删除分支，多人实现不同的功能，可以创建多个分支进行开发，之后进行分支合并，这种方式使开发变得快速、简单、安全。

### 1.2.Git优缺点

　　**优点**：

- 适合分布式开发，强调个体。
- 公共服务器压力和数据量都不会太大。
- 离线工作、速度快、灵活。
- 任意两个开发者之间可以很容易的解决冲突

　　**缺点**：

- 不符合常规思维。
- 代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息

### 1.3.版本控制由来

**本地版本控制系统**

![image-20211116152657889](git.assets/image-20211116152657889.png)



集中式版本控制工具：CVS、VSS、**SVN**等。

![image-20211116152738711](git.assets/image-20211116152738711.png)

分布式版本控制工具：**Git**、Mercurial、Bazaar、Darcs等。

![image-20211116152805955](git.assets/image-20211116152805955.png)

### 1.4.git的三种状态

- **modified：** 工作树中文件的副本已被编辑，并且与存储库中的最新版本不同。
- **staged：** 已经修改的文件，已添加到已更改文件列表中，以作为一个集合进行提交，但尚未提交。
- **committed：** 已经修改的文件，已提交到本地存储库。

<img src="git.assets/三种状态.png" alt="三种状态" style="zoom: 50%;" />

## 2.git初始化

使用git包随附的git-prompt.sh脚本，并将以下行添加到~/.bashrc中：

```shell
source /usr/share/git-core/contrib/completion/git-prompt.sh
export GIT_PS1_SHOWDIRTYSTATE=true
export GIT_PS1_SHOWUNTRACKEDFILES=true
export PS1='[\u@\h \W$(declare -F __git_ps1 &>/dev/null && __git_ps1 " (%s)")]\$ '

```

如果当前目录位于Git工作树中，则改工作树的当前Git分支的名称将显示在括号中。

提示符将提示以下内容：

+ (branch *) 表示在工作树中存在修改的跟踪文件 
+ (branch +) 表示在工作树中存在使用git add修改和暂存的跟踪文件
+ (branch %)表示在工作树中存在未跟踪的文件
+ 组合标记，例如(branch *+)

### 2.1.设置用户全局变量

由于Git用户经常修改多个项目，因此Git会在每次提交时记录用户的姓名和电子邮件地址。这些可以在项目级别定义，也可以为用户设置全局默认值。

git config --global命令控制这些设置，并将设置保存在~/.gitconfig文件中来管理用户所有git项目的默认设置。

```shell
$ git config --global user.name Ricardo_mao
$ git config --global user.email maochaomin2012@163.com
$ git config --global -l
core.editor="D:\Microsoft VS Code\bin\code.cmd" --wait
user.name=Ricardo_mao
user.mail=maochaomin2012@163.com

$ cat ~/.gitconfig
[core]
        editor = \"D:\\Microsoft VS Code\\bin\\code.cmd\" --wait
[user]
        name = Ricardo_mao
        mail = maochaomin2012@163.com

```

> git help

```shell
$ git help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)#开始工作区
   clone             #Clone a repository into a new directory  将存储库克隆到新目录中
   init             # Create an empty Git repository or reinitialize an existing one 创建一个空的Git存储库或重新初始化现有的Git存储库

work on the current change (see also: git help everyday)
   add               # Add file contents to the index  将文件内容添加到索引中
   mv                # Move or rename a file, a directory, or a symlink 移动或重命名文件、目录或符号链接
   restore           # Restore working tree files 恢复工作树文件
   rm                # Remove files from the working tree and from the index  从工作树和索引中删除文件
   sparse-checkout   # Initialize and modify the sparse-checkout 初始化并修改稀疏签出

examine the history and state (see also: git help revisions) #检查历史和现状
   bisect            #Use binary search to find the commit that introduced a bug 使用二进制搜索查找引入错误的提交
   diff              #Show changes between commits, commit and working tree, etc 显示提交、提交和工作树等之间的更改
   grep              #Print lines matching a pattern  打印与图案匹配的线条
   log               #Show commit logs   显示提交日志
   show              #Show various types of objects  显示各种类型的对象
   status            #Show the working tree status  显示工作树状态

grow, mark and tweak your common history
   branch            List, create, or delete branches
   commit            Record changes to the repository
   merge             Join two or more development histories together
   rebase            Reapply commits on top of another base tip
   reset             Reset current HEAD to the specified state
   switch            Switch branches
   tag               Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch             Download objects and refs from another repository
   pull              Fetch from and integrate with another repository or a local branch
   push              Update remote refs along with associated objects

```

