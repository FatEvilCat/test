**本文件是progit相关的学习记录**  

**Github相关的配置**

https://blog.csdn.net/buknow/article/details/80325986

**Github官网的使用说明**

echo "# test" >> README.md （新建了一个README）

git init  （没有仓库需要先初始化，当然已有仓库可以直接从git remote开始）

git add README.md  （暂存所有要提交的文件）

git commit -m "first commit" 

git remote add origin https://github.com/FatEvilCat/test.git  

git push -u origin master   （-u参数可以使得之后可以省略，用git push代替git push origin master）             

**git push卡在writing objects**

这是因为有较大的文件导致的，解决办法如下

https://blog.csdn.net/xfxf0520/article/details/86516885

# 第一章 起步

### 1. 版本控制

发展：本地版本控制→集中化的版本控制→分布式版本控制

​         	自己电脑上       在一个服务器上         每次clone都备份了代码

### 2. git基础

其他系统每次变更时保存变化的文件；Git保存文件时每次保存所有的文件，但是没有变化的文件在新版本中以**链接指向旧版本中的文件**保存。（快照流）

所有操作都是再本地进行。

校验和确保传输文件的完整性

Git一般只添加文件，而不删除

### 3. Git文件的三种状态

- 已提交（committed），保存在本地数据库中
- 已修改（modified），修改了文件，还没保存
- 已暂存（staged），对修改的文件做了标记，使之包含在下次提交的文件中

一般工作流程如下：

1. 在工作目录中修改文件
2. 暂存文件，将文件的快照放到暂存区域
3. 提交更新

### 4. 初次运行Git前的配置

1. config文件

2. 设置用户名和邮箱

   ```
   $ git config --global user.name "John Doe"
   $ git config --global user.email johndoe@example.com 
   ```

3. 文本编辑器，默认是Vim



# 第二章 Git基础

本章包含各种基础命令

### 1.获取Git仓库（两种）

**对现有项目进行管理**

得到一个Git仓库

1. 初始化

```
$ git init
```

在当前目录下创建一个.git的子目录

2. 跟踪文件

```
$ git add *.c
$ git add LICENSE
```

对指定文件进行跟踪

3. 提交

```
$ git commit -m 'initial project version'
```

-m参数是注释

**克隆已有的仓库**

```
$ git clone [url]
```

自定义名字

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

### 2. 记录每次更新

**查看当前文件状态**

```
$ git status
```

| 操作                      | 状态                                    |
| ------------------------- | --------------------------------------- |
| 新建一个文件              | Untracked files（未跟踪）               |
| git add（开始跟踪）       | Changes to be committed（暂存）         |
| 修改一个已跟踪的文件      | Changes not staged for commit（未暂存） |
| git add（已跟踪文件暂存） | Changes to be committed（暂存）         |

git add 指令的两个功能

注意，每次修改后要用git add把文件暂存起来

```
$ git status -s（或者 --short）
```

打印git status的简化版

??未跟踪，A暂存，M修改

**忽略文件，不纳入Git管理**

创建一个.gitignore的隐藏文件，在里面列出需要忽略的文件

```
$ cat .gitignore
*.[oa]
*~
```

第一行，所有.o或.a的文件（编译过程中产生）。第二行，波浪符结尾的，如Emacs用来保存副本（P40有更复杂的例子）

**查看具体的修改**

已修改，尚未暂存的变动

```
$ git diff
```

已暂存，准备下次提交的变动（二选一）

```
$ git diff --staged

$ git diff --cached
```

**提交更新**

注意每次提交前用 git status 检查一下，把未暂存的用 git add 暂存。

```
$ git commit
```

同时记录提交说明，使用 git commit -v 可以记录具体修改内容

```
$ git commit -m "Story 182: Fix benchmarks for speed"
```

使用-m参数，将说明内容同时提交

提交后可以看到提交的分支，校验和，修改的文件

**跳过暂存提交**

```
$ git commit -a -m 'added new benchmarks'
```

直接提交跟踪过的文件（修改的文件），跳过暂存的步骤

**移除文件**

$ git rm <文件名>

注意和 rm 有区别，rm是实际在目录中移除，git rm是在git仓库的清单中移除

-f 强制删除

**移动文件**

```
$ git mv file_from file_to
```

相当于

```
$ mv README.md README

$ git rm README.md

$ git add README
```



### 3.查看提交历史

```
$ git log
```

下面展示常用参数（所有参数见P50）

-p 显示每次提交内容的差异; 

-\<n>，最近的n次提交; --since, --until 

--stat 简略的统计信息;

--pretty 调整格式; 

--graph 使用一些字符串展示分支、合并历史

-S 仅显示添加或移除某些字符串的提交

#### 4.撤销操作

```
$ git commit --amend
```

提交后发现有文件忘记上传了，相当于覆盖，会进入编辑器，重新编辑注释内容

```
$ git reset HEAD <file>
```

取消暂存

```
$ git checkout -- <file>
```

撤销修改（针对未暂存文件）

### 5.使用远程仓库

**查看远程仓库**

```
$ git remote -v
```

可以列出多个远程仓库的合作者，以及状态（push, fetch）

**添加远程仓库**

```
$ git remote add <shortname> <url>
```

当添加后，可以用shortname代替整个URL，例如 $ git remote add pb https://github.com/paulboone/ticgit 后，可用 $ git fetch pb 获得数据。

```
$ git fetch [remote-name]
```

访问远程仓库，并拉取所有自己没有的数据。

默认 git clone的仓库以origin为简写，当使用 $ git fetch origin 后可以抓取新推送的所有工作。

git fetch不会自动合并或修改当前的工作，git pull可以合并到当前分支。

**推送到远程仓库**

分享自己的项目

```
$ git push [remote-name] [branch-name]  
```

$ git push origin master  每次只能一个人推送，否则后推送的人需要先拉取并合并字迹的工作后才能推送

**展示远程仓库**

```
$ git remote show [remote-name]  
```

**远程仓库移除和重命名**

```
$ git remote rename  <old_name> <new_name>

$ git remote rm  <name>
```

当重命名后，如果有引用旧名字的会自动更新成新名字

### 6.打标签

打上标签，以示重要，比如v1.0

```
$ git tag

$ git tag -l 'v1.8.5*'  
```

查找感兴趣的系列

**附注标签**

```
$ git tag -a v1.4 -m 'my version 1.4'  
```

使用 -m添加注释内容

**轻量标签**

```
$ git tag v1.4-lw  
```

不使用 -a, -s, -m 等任何选项

**后期补上标签**

```
$ git tag -a v1.2 9fceb02  
```

需要再末尾写上校验和（或部分）

**共享标签**

创建完标签后需要推送标签到共享服务器上

```
$ git push origin [tagname]  
```

推送一个标签

```
$ git push origin --tags  
```

推送所有的标签

### 7.别名

```
$ git config --global alias.unstage 'reset HEAD -- ‘
```

当起完别名后，以下两个命令等价

```
$ git unstage fileA
$ git reset HEAD -- fileA  
```

当需要添加外部命令时

```
$ git config --global alias.visual '!gitk'  
```



# 第三章 Git分支