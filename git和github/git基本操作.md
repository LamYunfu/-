# git基本操作

### git分支

#### 1.基本操作

* 查看当前所有分支

  `git branch`

* 创建分支

  `git branch feature-A`

* 切换分支

  `git checkout feature-A`

* 创建新的分支并切换到该分支下

  `git checkout -b feature-A`

* 切换回上一个分支

  `git checkout -`

* 查看当前工作树和暂存区的差别

  `git diff`

* 查看工作树和最新提交的差别

  `git diff HEAD`

---

#### 2.分支合并

* 切换到主分支

* 在主分支执行合并命令

  `git merge --no-ff feature-A`

  > --no-ff 参数的作用是禁止快进式合并，所谓快进式合并，就是比如说feature-A是master分支的下游分支，当没有--no-ff参数时，会导致移动master的指针直接指向feature-A分支，而不会生成新的合并分支，这样会“污染”master分支，当在master分支上进行版本回退的时候会回退到feature-A分支的上一级分支。

* 录入合并信息，关闭编辑器并保存

* 以图表形式查看日志

  `git log --graph`

---

#### 3.更改提交的操作

* git reset 回溯历史版本

  `git reset --hard 目标时间点的哈希值`

* `git log `命令只能查看以当前状态为终点的历史日志, `git reflog` 命令能够查看当前仓库所有的操作日志，在日志中找出回溯历史之前的哈希值。通过 `git reset --hard`命令恢复到回溯历 史前的状态
* `git commit --amend`  修改上一条提交信息
* 通过 git reset --hard命令恢复到回溯历 史前的状态
* git add命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已经跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态。
* git commit -am "..." 可以用于提交跟踪过的文件，省略这个文件执行git add ... 命令



**`git rebase` 合并提交历史**





