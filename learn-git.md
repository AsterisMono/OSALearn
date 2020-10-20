# Git
## 什么是Git？
分布式版本控制系统，能记录文件的改动，同时对分布式的开发提供一套健壮的流程

`fork->commit->push->pull request->merge`

## 建立Git仓库
`git init`

`Initialized empty Git repository in /home/catalysa/OSALearn/.git/`

## 添加文件到Git库：（到暂存区）
`git add readme.md`

删除文件：

`git rm readme.md`

## 提交一个Commit
提交 Commit ：确认到上一个 commit 为止做出的改动

`git commit -m "Initial commit"`

## 版本回退
*使用`git log`命令查看提交日志。*

`git reset --hard HEAD^`

or

`git reset --hard 1094a`

> HEAD 代表当前版本，一个^代表向前一个版本，多的时候使用HEAD~n。

> 使用 commit hash 时只需要输入前几位

## 回到未来
如果关闭了窗口，忘记新 commit 的 hash ，使用
`git reflog`查看（记录的每一次命令）。

## 工作区和暂存区
工作区：当前工作文件夹

暂存区：`git add`将文件提交到暂存区，`git commit`提交更改（到当前分支）

使用`git status`检查工作区状态

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

## Git工作流
`add->commit` 本地工作流

commit：提交的是 add ，不是文件修改

同时进行两次修改时要么两次 add 后commit，要么 add-commit-add-commit

> 查看工作区和**版本库中**最新版本区别：
>
> `git diff HEAD -- readme.md`

## 丢弃工作区的修改
### 1. 在工作区，未提交到暂存区

`git checkout -- readme.md`

回到最近一次`add`或者`commit`时的状态

### 2. 已提交到暂存区，未 commit

`git reset HEAD readme.md`

将暂存区的修改撤销，重新放回工作区

> 这个命令也可以用来把文件回退到某个版本。

### 3. 已经 commit
`git revert HEAD`

> 创建一个撤销上一个 commit 的新 commit 。

## 远程仓库
### Push
`git remote add origin URI`

第一次推送到远程：

`git push -u origin branch`

> -u 把本地分支和远程分支关联起来，今后只需使用 git push origin branch 。

### Clone
`git clone URI`

## 分支管理

使用分支：在不伤害 master 分支的情况下进行安全的操作

```
                  new branch
       (branch)/-------------\(merge)
master--------/               \---------master+
```

### 建立分支
`git branch new-branch`
> 不加参数的 `git branch` 会列出所有分支。

### 切换分支
`git switch new-branch`

or

`git checkout new-branch`

### 合并分支
`git merge branch-to-merge`
合并到**当前**分支。
> 显示 Fast-forward：直接将 master 指向 branch-to-merge ，速度非常快，但删除 branch 后会丢失分支信息。

### 删除已经合并的分支
`git branch -d branch-merged`

### 分支冲突
如果两个 commit 修改了同一文件的同一部分，那么会产生分支冲突。

```
Auto-merging readme.md
CONFLICT (content): Merge conflict in readme.md
Automatic merge failed; fix conflicts and then commit the result.
```
此时在工作区中查看 readme.md，git 会标记两个分支的各自内容，并要求你给出解决方案。

修改后`git add`，`git commit`进行提交。

### 分支管理：分支信息的忠实记录
关闭`Fast forward`来保留已合并的分支信息：

`git merge --no-ff branch1`

使用`git log`查看分支历史时，可以看到曾经的分支，不会丢分支信息。

### git stash
一个分支的修改已经暂存/尚未暂存，需要临时切换到另一个分支工作。

使用`git stash`保存现场以便未来使用：

`git stash pop`弹出 stash 内容，或者`git stash apply`+`git stash drop`。

> `git stash list` 会列出所有的 stash。可以按列表中格式恢复或删除指定的 stash。

### 在 branch 间复制 commit
常用于在 master 修复 bug 后复制给 dev 分支。

`git checkout dev`

`git cherry-pick commit-hash`

会自动生成新的 commit 进行修复。

### 分支管理策略
> 按层次高低排序。

master 分支：主要分支，成品代码。

dev 分支：正在开发的中间成品（或测试版）。

feature / bug 分支：添加的功能或者修复的bug，单开分支，结束后 merge 到 dev 分支。

### 远程分支策略
模拟加入多人协作。

> 查看远程库信息：`git remote -v`

`git clone`时，会自动把本地 master 和 origin master 相关联，但其他分支没有关联。

`git checkout -b dev origin/dev`来拉取远程 dev 分支并建立关联。

多人协作时，你的本地分支可能落后 origin ，使用`git pull`拉取远程分支的最新版本。

> 手动关联本地与远程分支：`git branch --set-upstream-to=origin/dev dev`

### git rebase
先 pull 他人更改再 push origin 时，提交历史会分叉，很难看。

使用`git rebase`把本地提交移动到 origin/branch 的最新提交之后。

> 发生在 ahead of master 的 commit 和你的 commit 发生冲突的时候。
>
> 使用 rebase 返回冲突处进行解决，然后 git add， 最后 git rebase --continue。

> *在rebase的过程中，也许会出现冲突(conflict). 在这种情况，Git会停止rebase并会让你去解决 冲突；在解决完冲突后，用"git-add"命令去更新这些内容的索引(index), 然后，你无需执行 git-commit,只要执行:*
>
>*$ git rebase --continue*
>
>*这样git会继续应用(apply)余下的补丁。*
>
> Ref:[Gitbook](http://gitbook.liuhui998.com/4_2.html)