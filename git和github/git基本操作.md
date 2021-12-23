# git基本操作

### git分支

#### 1.基本操作

* 查看当前所有分支

  `git branch`

  `git branch -a ` 同时查看本地分支与远程分支情况

* 创建分支

  `git branch feature-A`

* 切换分支

  `git checkout feature-A`

* 删除分支

  `git checkout -d (branchname)`

* 创建新的分支并切换到该分支下

  `git checkout -b feature-A`

* 切换回上一个分支

  `git checkout -`

* 查看当前工作树和暂存区的差别

  `git diff`

* 查看工作树和最新提交的差别

  `git diff HEAD`

* 从暂存区中移除文件

  `git rm filename`

---

#### 2.分支合并

* 切换到主分支

* 在主分支执行合并命令

  `git merge --no-ff feature-A`

  > --no-ff 参数的作用是禁止快进式合并(fast forward)，所谓快进式合并，就是比如说feature-A是master分支的下游分支，当没有--no-ff参数时，会导致移动master的指针直接指向feature-A分支，而不会生成新的合并分支，这样会“污染”master分支，当在master分支上进行版本回退的时候会回退到feature-A分支的上一级分支。

* 录入合并信息，关闭编辑器并保存

* 以图表形式查看日志

  `git log --graph`

* 以简洁一行的方式来查看日志

  `git log --oneline`

* 用--reverse参数来逆向显示所有日志。

  `git log --reverse`

* 查看指定文件的修改记录

  `git blame <file>`

---

#### 3.更改提交的操作

* git reset 回溯历史版本

  `git reset --hard 目标时间点的哈希值`

  这个操作会将当前分支的指针指向特定时间点分支状态的哈希值

* `git log `命令只能查看以当前状态为终点的历史日志, `git reflog` 命令能够查看当前仓库所有的操作日志，在日志中找出回溯历史之前的哈希值。通过 `git reset --hard`命令恢复到回溯历 史前的状态
* `git commit --amend`  修改上一条提交信息
* 通过 git reset --hard命令恢复到回溯历 史前的状态
* git add命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已经跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态。
* git commit -am "..." 可以用于提交跟踪过的文件，省略这个文件执行git add ... 命令



**`git rebase` 合并提交历史**

* 合并多个commit为一个完整的commit

这里我们使用命令:

```git
git rebase -i  [startpoint]  [endpoint]
```

其中`-i`的意思是`--interactive`，即弹出交互式的界面让用户编辑完成合并操作，`[startpoint]`  `[endpoint]`则指定了一个编辑区间，如果不指定`[endpoint]`，则该区间的终点默认是当前分支`HEAD`所指向的`commit`(注：该区间指定的是一个前开后闭的区间)。
 在查看到了log日志后，我们运行以下命令：

```undefined
git rebase -i 36224db
```

或:

```undefined
git rebase -i HEAD~3 
```

**将某一段commit粘贴到另外一个分支上**

当我们项目中存在多个分支，有时候我们需要将某一个分支中的一段提交同时应用到其他分支中，就像下图：

![img](https:////upload-images.jianshu.io/upload_images/2147642-0de010746cb78401.png?imageMogr2/auto-orient/strip|imageView2/2/w/808/format/webp)


 我们希望将develop分支中的C~E部分复制到master分支中，这时我们就可以通过rebase命令来实现（如果只是复制某一两个提交到其他分支，建议使用更简单的命令:`git cherry-pick`）。
 在实际模拟中，我们创建了master和develop两个分支:
**master分支:**

![img](https:////upload-images.jianshu.io/upload_images/2147642-c41f60d26b00cdfc.png?imageMogr2/auto-orient/strip|imageView2/2/w/443/format/webp)

**develop分支:**

![img](https:////upload-images.jianshu.io/upload_images/2147642-8519a024c88129c5.png?imageMogr2/auto-orient/strip|imageView2/2/w/455/format/webp)

我们使用命令的形式为:





```css
    git rebase   [startpoint]   [endpoint]  --onto  [branchName]
```

其中，`[startpoint]`  `[endpoint]`仍然和上一个命令一样指定了一个编辑区间(前开后闭)，`--onto`的意思是要将该指定的提交复制到哪个分支上。
 所以，在找到C(90bc0045b)和E(5de0da9f2)的提交id后，我们运行以下命令：



```undefined
    git  rebase   90bc0045b^   5de0da9f2   --onto master
```

注:因为`[startpoint]`  `[endpoint]`指定的是一个前开后闭的区间，为了让这个区间包含C提交，我们将区间起始点向后退了一步。
 运行完成后查看当前分支的日志:

![img](https:////upload-images.jianshu.io/upload_images/2147642-de397671caac1966.png?imageMogr2/auto-orient/strip|imageView2/2/w/488/format/webp)

可以看到，C~E部分的提交内容已经复制到了G的后面了，大功告成？NO！我们看一下当前分支的状态:

![img](https:////upload-images.jianshu.io/upload_images/2147642-cfd21fdb1e4038bc.png?imageMogr2/auto-orient/strip|imageView2/2/w/439/format/webp)

当前HEAD处于游离状态，实际上，此时所有分支的状态应该是这样:

![img](https:////upload-images.jianshu.io/upload_images/2147642-a3bbfea6d760f64a.png?imageMogr2/auto-orient/strip|imageView2/2/w/755/format/webp)


 所以，虽然此时HEAD所指向的内容正是我们所需要的，但是master分支是没有任何变化的，`git`只是将C~E部分的提交内容复制一份粘贴到了master所指向的提交后面，我们需要做的就是将master所指向的提交id设置为当前HEAD所指向的提交id就可以了，即:

```undefined
      git checkout master
      git reset --hard  0c72e64
```

![img](https:////upload-images.jianshu.io/upload_images/2147642-003361cb0305c094.png?imageMogr2/auto-orient/strip|imageView2/2/w/689/format/webp)

此时我们才大功告成！

---

#### 4. 推送至远程仓库

**git push** 命用于从将本地的分支版本上传到远程并合并。

命令格式如下：

```
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```
git push <远程主机名> <本地分支名>
```

在推送至远程仓库的过程中，可以添加-u参数

```
git push -u origin master
```

-u参数可以在推送的同时，将 origin 仓库的 master 分 支设置为本地仓库当前分支的 upstream（上游）。添加了这个参数，将来运行 git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从 origin 的 master 分支获取内容，省去了另外添加参数的麻烦。

#### 5. 从远程仓库获取

* 获取远程仓库

`git clone 仓库地址`

执行 git clone命令后我们会默认处于 master 分支下，同时系统会自动将 origin 设置成该远程仓库的标识符。也就是说，当前本地仓库 的 master 分支与 GitHub 端远程仓库（origin）的 master 分支在内容上是完全相同的

* 获取远程分支

`git checkout -b feature-D origin/feature-D`

\- b 参数的后面是本地仓库中新建分支的名称。为了便于理解，我 们仍将其命名为 feature-D，让它与远程仓库的对应分支保持同名。新建分支名称后面是获取来源的分支名称。例子中指定了 origin/feature-D， 就是说以名为 origin 的仓库（这里指 GitHub 端的仓库）的feature-D 分支为来源，在本地仓库中创建 feature-D 分支。

---

#### 5. HEAD^和HEAD~

`^`和`~`是2个很有意思的字符，配合使用可表示祖宗十八代。任你给一个节点（HEAD 或 哈希值），都能顺藤摸瓜，找到其祖先是谁。

网友通常这么解释：

- `^`：表示第几个父/母亲 ——　git存在多个分支合并的情况，所以不只有1对父母亲
- `~`：表示向上找第几代，相当于连续几个 `^`

比如有这样一个库：



```undefined
          b1(HEAD)
          |
C4 ------ C6
         /
   --- C5

自己: C6 = HEAD^0         = HEAD~0         = C6^0       = C6~0    
父亲: C4 = HEAD^1 = HEAD^ = HEAD~1 = HEAD~ = C6^1 = C6^ = C6~1 = C6~ 
母亲: C5 = HEAD^2                          = C6^2 
```

---

#### 6. 查看提交历史

**git log 常用选项**

* 显示每次提交的差异

  `git log -p/--patch -2`  显示最近两次提交的差异

* 看到每次提交的简略统计信息，可以使用 `--stat` 选项：

  `git log --stat`

* 用不同于默认格式的方式展示提交历史

  `git log --pretty=oneline`

---

#### 7. 撤销操作

* 重新提交覆盖上一次提交

  `git commit --amend`

* 取消暂存区的某个文件的暂存状态

  `git reset HEAD <file>`

* 撤销对于某个文件的修改，将其还原成上次提交时候的样子

  `git checkout -- <file>`

---

#### 8. 远程仓库

* 查看远程仓库使用的 Git 保存的简写与其对应的 URL

  `git remote -v`

* 添加一个远程仓库

  `git remote add <shortname> <url>`

* 