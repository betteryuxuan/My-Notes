﻿

GIT

@[toc]

# 概述

##  为什么选择git?

Git 是一个强大、灵活且高效的版本控制系统，被广泛用于软件开发和其他领域，是开发者不可或缺的工具之一

>**为什么选择git?**
>
>1. **版本控制：** Git 提供了强大的版本控制功能，可以追踪文件的每一次修改，记录项目的演变历史。这使得开发者可以随时回滚到之前的版本，比较不同版本之间的差异，以及分析项目的发展轨迹。
>2. **分布式开发：** Git 是一种分布式版本控制系统，每个开发者都可以在本地拥有完整的项目副本，不依赖于中央服务器。这样可以在离线状态下进行工作，并允许灵活的分支管理和合并。
>3. **协同工作：** Git 提供了强大的协同工作机制，多人可以同时在一个项目上工作，而不会干扰彼此的修改。分支合并、冲突解决等功能使得团队协作更加高效。
>4. **灵活的分支管理：** Git 的分支管理是其强项之一，开发者可以轻松创建、合并、删除分支，实现不同功能的并行开发，而不会影响主干代码。
>5. **远程仓库：** Git 支持将代码托管到远程仓库，如 GitHub、GitLab、Bitbucket 等平台。这使得团队能够远程协作、分享代码，同时享受到版本控制和备份的好处。

## 版本控制工具

> a、集中式版本控制工具
>
> 集中式版本控制工具，版本库是集中存放在中央服务器的，team里每个人work时从中央服务器下载代码，是必须联网才能工作，局域网或互联网。个人修改后然后提交到中央版本库。
>
> 举例：SVN和CVS
>
> b、分布式版本控制工具
>
> 分布式版本控制系统没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样工作的时候，无需要联网了，因为版本库就在你自己的电脑上。多人协作只需要各自的修改推送给对方，就能互相看到对方的修改了。
>
> 举例：Git



# 基本配置
* 设置用户信息

```bash
git config --global user.name “zhangsan”

git config --global user.email "123456@qq.com"
```

* 查看配置信息

```bash
git config --global user.name

git config --global user.email
```

* **config**的三个作用域

`$ git config --local` 只对单个仓库有效

`$ git config --global` 对当前所有仓库有效(常用)

`$ git config --system` 对系统所有登录用户有效

##  建立Git仓库

### 场景一

已有项目纳入git管理

```bash
$ cd 项目代码所在文件夹`
$ git init
```

### 场景二

新建项目用git管理
```bash
$ cd 某个文件夹`
$ git init your_project
$ cd your_project
```
# 基础操作

## 仓库管理
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/9d82e45288d74ccab91391c6f9dd9317.png#pic_center)




> 使用 `git add` 将修改的文件添加到暂存区。
>
> 使用 `git commit` 将暂存区的内容提交到本地库。



* **状态转换**

1. `git add `(工作区 --> 暂存区)

2. `git commit `(暂存区 --> 本地仓库)

`git commit -am `命令用于提交所有已经被Git管理的文件,同时添加了提交消息。相当于分别执行了`git add .`和`git commit -m "message"`两个步骤。
> -am 选项只会提交已经被 Git 管理的文件，对于未被 Git 管理的新文件，你需要手动使用 `git add` 将它们添加到暂存区。

* **查看修改的状态（status）**

作用：查看的修改的状态（暂存区、工作区）

命令形式：`git status`



* **添加工作区到暂存区(add)**

作用：添加工作区一个或多个文件的修改到暂存区

命令形式：`git add 单个文件名|通配符`

将所有修改加入暂存区：`git add .`



* **提交暂存区到本地仓库(commit)**

作用：提交暂存区内容到本地仓库的当前分支

命令形式：`git commit -m '注释内容'`




* **查看提交日志(log)**

作用:查看提交记录

命令形式：`git log [option]`

>--all 显示所有分支
>
>--pretty=oneline 将提交信息显示为一行
>
>--abbrev-commit 使得输出的commitId更简短
>
>--graph 以图的形式显示



* **版本回退**

作用：版本切换

命令形式：`git reset --hard commitID`

`commitID` 可以使用 `git-log `或 `git log `指令查看



* **查看版本信息**

`git reflog`

这将显示一个列表，其中包含了本地仓库最近的操作历史，包括提交、分支切换等。每个操作都有一个对应的 HEAD 指针位置和一个唯一的哈希值。

如果你需要查找丢失的提交或还原已删除的分支，可以使用 `git reflog` 找到相应的哈希值，然后使用其他 Git 命令进行操作，比如：

```
# 恢复到指定的哈希值
git reset --hard <commit-hash>

# 创建分支并切换到指定的哈希值
git checkout -b <new-branch> <commit-hash>
```



* **修改最近一次的提交**

作用：执行 `git commit --amend` 将会打开文本编辑器（通常是你配置的默认编辑器），让你修改最近一次的提交信息。你可以修改提交信息并保存退出编辑器，Git 将会更新最近的提交，并使用你修改后的提交信息。

命令形式：`git commit --amend`


* **`git rebase`**
交互式`rebase` `（interactive rebase）`是 Git 中一个非常强大的工具，它允许你以交互方式重新排列、编辑和合并提交记录。通过交互式`rebase`，你可以轻松地从一系列提交记录中选择特定的提交，并将它们重排或者合并到目标分支上。

下面是使用交互式 rebase 的一般步骤：
1. 执行交互式 rebase：
```bash
git rebase -i <commit>
```
这里的` <commit> `是你希望从哪个提交开始进行`rebase`的提交的哈希值或者引用。如果你想从当前分支的`HEAD`开始，可以省略这个参数。

2. Git 会打开一个文本编辑器，显示一个提交记录列表，类似于下面这样：
```sql
pick abc123 Commit message 1
pick def456 Commit message 2
pick hij789 Commit message 3
```
每一行都以 pick 开头，后面跟着提交的哈希值和提交信息。

3. 在编辑器中，你可以修改这个列表以达到你的目的。你可以：
* 将`pick`修改为`edit`，这样会在应用到这个提交时暂停`rebase`进程,让你有机会修改提交的内容或者重新提交。
* 将`pick`修改为`squash`或者`fixup`，这样会将当前提交合并到前一个提交中，或者将当前提交合并到前一个提交并丢弃提交信息。
* 删除某些提交记录，以跳过它们。
* 调整提交记录的顺序。

> - `pick`：保留该提交，不做修改。
> - `reword`：保留该提交，并修改提交消息。
> - `edit`：保留该提交，并让你修改提交的内容。
> - `squash`：将该提交与前一个提交合并。
> - `fixup`：将该提交与前一个提交合并，但忽略该提交的提交消息。
> - `drop`：删除该提交。

4. 修改完毕后，保存并退出编辑器，Git 将会按照你的指示重播提交记录。当 rebase UI界面打开时, 你能做3件事:
* 调整提交记录的顺序
* 删除你不想要的提交
* 合并提交,允许你把多个提交记录合并成一个。  <br><br><br>

* **整理提交记录**
`git cherry-pick` 命令用于将指定提交的更改应用到当前分支的位置上。它的语法是：

```bash
git cherry-pick <commit-hash>...
```
` <commit-hash> `是要应用的提交的哈希值。
使用`git cherry-pick`可以很方便地将其他分支上的单个或多个提交应用到当前分支上，而无需将整个分支合并过来。这在需要单独引入特定变更而不影响其他变更时非常有用

* **比较工作区和暂存区的代码差异**：

使用 `git diff`命令可以比较工作区中的修改和暂存区中的内容之间的差异。例如：

```bash
git diff
```
这将显示工作区中未暂存的修改。

* **比较暂存区和最新提交（HEAD）的代码差异**：

使用 `git diff --staged`命令可以比较暂存区中的修改和最新提交（HEAD）之间的差异。例如：

```bash
git diff --staged
```

这将显示暂存区中即将提交的内容和最新提交之间的差异。

* **比较工作区和最新提交（HEAD）的代码差异**：

可以结合使用`git diff`和 `HEAD`参数来比较工作区和最新提交（HEAD）之间的差异。例如：

```bash
git diff HEAD
```

## 分支管理

 使用分支意味着你把工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

 **HEAD指向分支，分支指向版本号**

* **查看本地分支**

​	   创建本地分支

​	   命令：`git branch 分支名`

* **显示本地分支**

​	   命令：`git branch -v` 

* **切换分支**

​	   命令：`git checkout 分支名`

* **创建并切换分支**

​	   命令：`git checkout -b 分支名`

* **合并分支**(merge)

​	   一个分支上的提交可以合并到另一个分支

​	   命令：`git merge `

​	   把指定的分支（`分支名称`）上的修改合并到当前所在的分支

>`Fast-Forward`合并：
>如果要合并的两个分支之间没有分叉（即一个分支是另一个的直接后继），Git 可以通过简单地移动指针来完成合并，这种情况称为 Fast-Forward 合并。在这种情况下，合并后的提交历史形成一条直线，没有额外的合并提交。这种合并非常快速和简单，不会产生额外的提交。

> 普通合并：
> 如果要合并的两个分支之间有分叉，Git 就需要创建一个新的合并提交来将两个分支的历史合并在一起。在这种情况下，Git 会自动创建一个新的合并提交，将两个分支的历史整合在一起。合并提交包含了两个分支最后一个共同提交的内容，以及之后的各自的提交。

* **删除分支**

<u>	不能删除当前分支，只能删除其他分支</u>

​	   `git branch -d b1`删除分支时，需要做各种检查

​	   `git branch -D b1`不做任何检查，强制删除
* **强制修改分支位置**
	命令：`git branch -f`
	用于强制移动一个分支的指针到另一个提交上。：
```bash
git branch -f <branchname> <commit>
```
`<branchname> `是你想要移动的分支的名称。
`<commit> `是你想要将分支移动到的目标提交的引用，可以是提交的哈希值、分支名或相对引用。
* **撤销变更**
>撤销变更指的是将之前的代码更改回滚到之前的状态，使得代码库恢复到之前的版本或状态。

`git reset`：用来移动当前分支的指针位置，并且可以选择是否同时修改暂存区和工作目录。
有三种模式可以用于`git reset`命令：`--soft`、`--mixed` 和 `--hard`。
`--soft` 模式只会移动分支指针，不会修改暂存区和工作目录，这意味着变更仍然保留在暂存区和工作目录中，你可以重新提交它们。
`--mixed` 模式会移动分支指针，并且将变更从暂存区撤销到工作目录中，但不会删除工作目录中的变更。
`--hard` 模式会移动分支指针，并且将暂存区和工作目录中的变更全部删除，慎用，因为它会永久性地丢弃变更。
<u>在reset后，所做的变更还在，但是处于未加入暂存区状态</u>

`git revert`：
用于创建一个新的提交，该提交的内容是指定提交的相反变更。换句话说，它会撤销某次提交的变更，但是会保留该提交的历史记录。`git revert` 命令会创建一个新的提交，该提交的内容与指定提交的相反。这样做的好处是可以保留提交历史，避免破坏代码库的完整性。
通过这种方式，其他开发者可以从远程分支获取到撤销提交，并将其应用到自己的分支中，从而保持代码库的一致性，避免造成混乱和不必要的冲突。
```bash
# 创建一个新分支以防止直接在主分支上进行实验
git checkout -b fix-bug

# 使用 git revert 撤销之前的提交变更
git revert <commit-hash>

# 提交撤销更改
git commit -m "Revert previous commit"

# 将撤销更改推送到远程分支
git push origin fix-bug
```

* **标签（tags)**

Git 提供了一种称为标签（Tags）的功能，可以用来永久性地标识某个特定的提交。标签是指向某个特定提交的指针，它们不会随着新的提交而移动，因此非常适合用来标识软件发布的版本、修复重要的 Bug 或者添加新特性等重要的提交。
使用标签可以确保你可以在任何时候轻松地回到标记的提交状态，而不必担心它会移动或被覆盖。这对于软件发布非常有用，因为你可以在发布软件的同时创建一个标签，以便随时可以通过标签回到该版本。
可以使用 git tag 命令来创建一个新的标签，并指定要标记的提交。例如：
```bash
git tag v1.0 <commit>
```
这将创建一个名为 v1.0 的标签，并将其指向指定的提交。你也可以使用 -a 选项来创建一个带注释的标签，以便记录标签的相关信息。
```bash
git tag -a v1.0 -m "Release version 1.0" <commit>
```
创建标签后，你可以使用`git show <tag>`命令来查看标签的信息，以及标签所指向的提交。

* **描述当前提交位置**

`git describe`
专门设用来描述离你最近的锚点的命令（也就是标签）
git describe 的​​语法是：

`git describe <ref>`

`<ref> `可以是任何能被 Git 识别成提交记录的引用，如果你没有指定的话，Git 会使用你目前所在的位置`（HEAD）`
它输出的结果是
`<tag>_<numCommits>_g<hash>`
`tag`表示的是离`ref`最近的标签，`numCommits`是表示这个`ref`与`tag`相差有多少个提交记录，`hash`表示的是你所给定的`ref`所表示的提交记录哈希值的前几位。
当 ref 提交记录上有某个标签时，则只输出标签名称

* **文件的重命名**

​	   `$ git mv readme readme.md`
​	   等同于三个步骤
```bash
$ mv readme readme.md  # 在文件系统中重命名文件
$ git add readme.md   # 将重命名后的文件添加到 Git 暂存区
$ git commit -m "Rename readme to readme.md"
```

* **版本历史查看**

​	`	$ git log --oneline`简洁

​	`	$ git log -n4` 看所有分支最近四次

​	`		$ git log --graph` 图形化

* **图形化界面版本**

​	`$ gitk`

# .git目录

## 目录内容

>1. **branches：** 包含本地分支的信息。
>2. **hooks：** 包含客户端和服务器端的钩子脚本。这些脚本允许你在特定的 Git 事件发生时执行自定义操作。
>3. **info：** 包含一个全局的排除文件，用于指定哪些文件不应被纳入版本控制。
>4. **objects：** 包含所有的 Git 对象（提交、树、Blob 等）。
>5. **<u>refs</u> ：**存储引用（分支、标签等）的目录。
>6. **<u>config</u>：** 存储仓库的配置信息，如用户信息、别名等。
>7. **<u>HEAD</u>**： 指向当前所在分支的引用。
>8. **index：** 存储暂存区的内容，包括文件路径、SHA-1 哈希值等信息。
>9. **logs：** 包含引用的历史信息。
>10. **COMMIT_EDITMSG：** 存储上一次提交的提交消息。
>11. **FETCH_HEAD：** 包含最近的远程获取操作的信息。

---

以下是一些查看文件夹内容需要使用的相关指令

**显示文件内容：**

```bash
cat filename
```

**显示指定对象的类型:**

```bash
git cat-file -t <object-id>
```

其中 `<object-id>` 是对象的哈希值或引用（分支、标签等）。执行这个命令将显示指定对象的类型。

输出对象的类型，可能是 `commit`、`tree`（树对象）、`blob`（文件对象）等。

- **-t：** 显示对象的类型。

- **-s：** 显示对象的大小。

- **-p：** 显示对象的内容。

  ---

  

### HEAD
**HEAD 指针：** `HEAD` 是一个特殊的指针，它总是指向当前所在的本地分支的最新提交。如果你处于某个分支上，`HEAD` 就会指向这个分支。

   进入`.git`目录打开head

![head1](https://img-blog.csdnimg.cn/img_convert/cc904fc64a172c6b582c6027db512e0d.png)

切换分支后

![head2](https://img-blog.csdnimg.cn/img_convert/7c66b212befa0db8118353fbdc7289a6.png)

###  refs

![head3](https://img-blog.csdnimg.cn/img_convert/470907200c8d06fe76a5f7ab21cefa6c.png)
在Git中，`refs`目录是用于存储引用（references)的地方。引用是指指向Git对象的指针，常用于表示分支、标签等。`refs`目录中包含了几个子目录，每个子目录都用于存储特定类型的引用。

以下是`refs`目录中常见的子目录：

1. **heads**：这个子目录存储着本地分支的引用。每个文件代表一个分支，文件名就是分支的名称，文件内容是指向该分支最新提交的SHA-1值。例如，`refs/heads/master`文件存储着`master`分支的引用。
2. **tags**：这个子目录存储着标签（tag）的引用。每个文件代表一个标签，文件名就是标签的名称，文件内容是指向该标签所指向的提交对象的SHA-1值。例如，`refs/tags/v1.0`文件存储着`v1.0`标签的引用。
3. **remotes**：这个子目录存储着远程跟踪分支的引用。每个文件代表一个远程跟踪分支，文件名通常由远程仓库名和分支名组成，文件内容是指向该分支最新提交的SHA-1值。例如，`refs/remotes/origin/master`文件存储着`origin`仓库的`master`分支的引用。
4. **stash**：这个文件存储着stash的引用。Stash是用于保存工作目录和暂存区的临时状态的功能。`refs/stash`文件存储着stash栈顶的引用。

这些目录和文件构成了Git中引用的存储结构，它们帮助Git管理分支、标签、远程仓库和stash等重要信息。

### objects

`objects` 目录中包含以下子目录：

1. `info`：这个子目录存储一些额外的信息，比如 Git 仓库的一些配置信息。
2. `pack`：在 Git 中，当有很多松散的对象需要存储时，Git 会将这些对象打包成一个或多个 pack 文件，然后存储在这个子目录下。
3. 哈希命名的对象目录：这些目录是 Git 对象的实际存储位置，它们的名称是根据对象的哈希值计算而来的。例如，一个对象的哈希值为 `abcd1234`，那么它就会被存储在名为 `ab` 的子目录下，并且以 `cd1234` 作为文件名存储在其中。

在这些哈希命名的对象目录中，实际上存储了 Git 的四种基本对象类型：blob、tree、commit 和 tag。每个对象都是通过哈希算法计算出来的哈希值来命名和索引的。

![head4](https://img-blog.csdnimg.cn/img_convert/631e7130b89d3d9f70babfaa8b36d7cc.png)

文件名（2位字符）加上文件内储存的38位字符组成40个字符的十六进制字符串

利用 `git cat-file -t <object-id>`可以得出其类型

![head5](https://img-blog.csdnimg.cn/img_convert/d88893de5131a7b072649265fe5b99ad.png)

利用 `git cat-file -p <object-id>`打开其内容，发现`blob`类型文件

![head6](https://img-blog.csdnimg.cn/img_convert/b6287f2a9ca080aa087b256da36b13e1.png)

**tips**：Git 中的对象存储机制是基于内容的哈希值来决定对象的唯一性的，它们的 `Blob` 对象的哈希值也会相同，从而大大节省了存储空间。笔者的`file02`, `file03`, `file04`, `file05`都为空，文件内容相同，因此哈希值相同，这种机制使得 Git 在存储和比较文件时非常高效。

## git基本对象类型

在 Git 中，有四种基本对象类型，它们之间有一定的关系，构成了 Git 的对象模型。这四种对象类型分别是：

1. **blob（文件内容对象）：** Blob 对象存储了文件的内容。每个文件对应一个唯一的 blob 对象，通过文件内容的 SHA-1 哈希值来唯一标识。
2. **tree（目录结构对象）：** Tree 对象存储了目录结构和文件与对应 blob 对象之间的映射关系。它包含了一组文件名、文件类型（blob 或 subtree）、以及与之关联的 blob 对象或 subtree 对象的 SHA-1 哈希值。
3. **commit（提交对象）：** Commit 对象存储了一次提交的信息，包括作者、提交时间、提交说明以及指向一个树对象的引用。每个提交指向一个特定的树对象，表示该次提交的项目状态。
4. **tag（标签对象）：** Tag 对象存储了一个标签的信息，通常用于给某个提交打上一个有意义的标签。标签对象包含了标签名称、标签引用的对象（通常是一个 commit 对象）等信息。

- **Commit 对象与 Tree 对象的关系：** 每个 commit 对象都指向一个 tree 对象，表示该次提交的项目状态。
- **Tree 对象与 Blob 对象的关系：** 每个 tree 对象包含了一组文件名和对应的 blob 对象的 SHA-1 哈希值，表示文件和目录的结构。

![head7](https://img-blog.csdnimg.cn/img_convert/3b4e57c15c72061ee11768d8e61f4b23.jpeg)

> Tree 对象代表目录结构，每个节点可以指向一个 Blob 对象（文件）或另一个 Tree 对象（子目录），以此类推，形成了文件和目录之间的层级关系。

快照是什么？

> **快照**（snapshot）通常是指对整个项目状态的一次完整拍摄，包括所有文件和目录的当前状态。与传统的版本控制系统不同，Git 不仅仅记录每个文件的更改差异，而是创建了整个项目在某个时间点的快照。

# 分离头指针

“分离头指针”是指将HEAD指向一个特定的提交（commit），而不是指向分支的末端。这通常发生在你切换到一个特定的提交时，而不是切换到一个分支上。这种情况下，HEAD指针就会“分离”，因为它没有指向任何分支，而是直接指向一个提交。这可能会导致一些问题，因为你在分离头指针状态下进行的提交不会更新任何分支，而是直接在当前提交的基础上进行。

如果你不小心进入了分离头指针状态，想要回到一个分支上，可以简单地通过切换到想要的分支来解决：

```bash
git checkout <branch_name>
```

或者，如果你想在当前分离头指针状态下创建一个新的分支，可以使用以下命令：

```bash
git checkout -b <new_branch_name>
```

这样就会创建一个新的分支，并将其指向当前所在的提交。

分离头指针有时候是有用的，比如你想要查看或测试一个特定的提交，而不想在其基础上创建新的提交。但在大多数情况下，最好还是在一个具有明确名称的分支上进行工作，以避免混淆和不必要的麻烦。

# GIT团队协作

## 团队内协作

### 分支管理

在团队内协作中，分支管理是一个关键的方面。每个团队成员可以在本地创建自己的分支进行开发，而不影响主分支。一般而言，主分支（通常是 `main` 或 `master`）应该保持稳定且用于发布生产版本。团队成员可以在各自的分支上进行开发，然后通过合并请求（Pull Request）将自己的修改合并到主分支。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/38b1c2e3dff7439dbd91c55f9df85378.png#pic_center)


### 团队协作流程

1. **拉取最新代码：** 在开始工作之前，确保本地代码库是最新的。

   ```bash
   git pull origin main
   ```

2. **创建新分支：** 为了开发新功能或修复问题，创建一个新的分支。

   ```bash
   git checkout -b feature-branch
   ```

3. **进行开发：** 在新分支上进行开发工作，进行一系列的提交。

   ```bash
   git commit -m "feat: 新功能"
   ```

4. **同步主分支：** 定期将主分支的最新代码合并到当前分支，以保持同步。

   ```bash
   git pull origin main
   ```

5. **解决冲突：** 如果在合并时发生冲突，需要手动解决冲突。

   ```bash
   解决冲突后，添加修改并提交
   git add .
   git commit -m "fix: 解决合并冲突"
   ```

6. **合并到主分支：** 完成开发后，将当前分支合并到主分支。

   ```bash
   git merge feature-branch
   ```

7. **推送到远程仓库：** 将本地的修改推送到远程仓库。

   ```bash
   git push origin main
   ```

8. **创建合并请求（Pull Request）：** 在远程仓库创建一个合并请求，请求将当前分支的修改合并到主分支。

   ```bash
   # 创建并切换到主分支
   git checkout main
   
   # 确保本地主分支是最新的
   git pull origin main
   
   # 切换回开发分支
   git checkout feature-branch
   
   # 将主分支合并到开发分支
   git merge main
   ```

### 远程仓库操作

1.创建远程仓库

![github](https://img-blog.csdnimg.cn/img_convert/94e1cba12fc4f53519a6b0c64a8d6046.png)

2.初始化

![github2](https://img-blog.csdnimg.cn/img_convert/db0456204c4ab6d1fc3de11d75f02457.png)

3.仓库创建完成后可以看到仓库地址

![git3](https://img-blog.csdnimg.cn/img_convert/ce31c267ec736fbb6d3624f6e2e6f34a.png)

### 常见Git远程仓库操作命令

1. **添加远程仓库,起别名：**

   ```bash
   git remote add <远程仓库名> <远程仓库URL>
   ```
   
2. **查看远程仓库列表：**

   ```bash
   git remote -v
   ```
   
3. **从远程仓库拉取更改到本地：**

   ```bash
   git pull <远程仓库名> <分支名>
   ```
   `git pull` 命令其实包含了`git fetch`和`git merge`两个步骤。它的作用是从远程仓库获取更新，并且将这些更新合并到当前分支上。
  `git fetch`：
git fetch 命令用于从远程仓库下载新的提交、分支和标签，但它不会自动合并这些更改到你当前的工作分支。你可以通过 `git merge [远程仓库名]/[分支名] `或者 `git rebase [远程仓库名]/[分支名]` 将远程分支的更新合并到你当前的分支。
4. **推送本地更改到远程仓库：**

   ```bash
   git push <远程仓库名> <本地分支名>:<远程分支名>
   ```
   
5. **查看远程仓库详细信息：**

   ```bash
   git remote show <远程仓库名>
   ```
   
6. **移除远程仓库：**

   ```bash
   git remote rm <远程仓库名>
   ```
   
7. **重命名远程仓库：**

   ```bash
   git remote rename <旧仓库名> <新仓库名>
   ```

8. **克隆远程仓库到本地：**

	```bash
	git clone <远程仓库URL>
	```

​	（1）拉取代码

​	（2）初始化本地仓库

​	（3）创建别名(默认"`origin`")		

> ​	  **克隆不需要登录账号**
>
> ​      **用户二克隆用户已仓库后，需要被用户一邀请加入仓库，用户二才可以推送(push)更改到github仓库**

## 跨团队协作

### Fork作用

> 1. **独立工作空间**：Fork 允许你在 GitHub 上复制一个项目到你自己的账号下，形成一个独立的工作空间。这个 Forked 项目与原始项目相互独立，你可以在其中进行修改、添加新功能，而不影响原始项目。
>
> 2. **贡献代码**：通过 Fork 一个项目，你可以在自己的 Forked 项目中进行修改，然后通过 Pull Request 向原始项目提交你的修改。这样原始项目的维护者可以审查你的修改，并决定是否将其合并到原始项目中。这是开源社区中贡献代码的常见方式之一。
>
> 3. **实验性修改**：如果你想尝试对一个项目进行修改，但又不确定是否会被原始项目接受，你可以先 Fork 这个项目，在自己的 Forked 版本中进行实验性的修改。这样做不会影响原始项目，同时也可以保留你的修改历史记录。
>
> 4. **独立版本管理**：Fork 的项目拥有独立的版本控制，你可以在自己的 Forked 项目中进行任意的提交、分支和版本管理，而不会影响原始项目的版本历史。



Fork 是 GitHub 中一种重要的协作方式，它允许开发者在保持原始项目的完整性的同时，对项目进行修改和贡献，促进了开源社区的发展和代码共享。

### Fork操作流程

`Fork` 操作的步骤如下：

1. **在 GitHub 上找到目标仓库：** 首先，你浏览到你感兴趣的 GitHub 仓库的页面。
2. **点击 "Fork" 按钮：** 在目标仓库的右上角，有一个名为 "Fork" 的按钮。点击它将会在你的 GitHub 账户下创建一个该仓库的拷贝。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/3e0447020c8f4e2abff14f55e9a18d27.png#pic_center)


3. **等待复制完成：** GitHub 将会在你的账户下创建一个新的仓库，其中包含原始仓库的所有代码、分支、提交历史等。你会被自动重定向到这个新仓库。
4. **在你的账户下修改代码：** 现在，你可以在你 Fork 的仓库中进行修改、添加新功能、修复问题等。

5. **向原始仓库提出合并请求（Pull Request）：** 如果你希望你的修改被原始仓库接受，你可以向原始仓库提交一个合并请求。这个请求会通知原始仓库的维护者审查你的更改，并决定是否将它们合并到原始仓库中。

> `Fork`的主要优势在于允许你贡献到其他项目而不需要直接访问它们的代码库。你可以在你自己的 Fork 下进行任何更改，而这些更改不会影响到原始仓库，直到你主动提出合并请求并得到原始仓库维护者的批准。开发者能够方便地参与到其他项目的开发中。

# SSH 密钥 

## SSH 密钥作用：

> 1. **身份验证：** SSH 密钥提供了一种更安全的身份验证方式。它使用密钥对进行验证，其中私钥保存在本地，而公钥被添加到 GitHub。这比基于用户名和密码的身份验证更加安全。
> 2. **免密码登录：** 一旦将 SSH 密钥添加到 GitHub，你就无需每次都输入密码来进行操作。这样可以减轻不必要的麻烦，并提高操作的效率。
> 3. **方便的 Git 操作：** 添加 SSH 密钥后，你可以方便地使用 SSH 协议进行 Git 操作，如克隆、推送、拉取等，而不再需要每次输入密码。
> 4. **安全性：** SSH 使用非对称加密，提供了更高的安全性。私钥储存在本地，不容易被他人获取，而公钥存储在 GitHub 上。

## 添加 SSH 密钥到 GitHub

1. 打开图示文件夹

![ssh1](https://img-blog.csdnimg.cn/img_convert/de4f5cec672f852f49439e61aa5f3f7c.png)

2. 打开`git bash`，输入下方命令生成`.ssh`文件夹

   ```bash
   $ ssh-keygen -t rsa -C <github登陆邮箱>
   ```
   ![ssh2](https://img-blog.csdnimg.cn/img_convert/e44d4ee97090b33bc46c64b6e13b67bc.png)

![ssh3](https://img-blog.csdnimg.cn/img_convert/943fa4daeed16847b78d29dbacd509e4.png)

4. 输入下方命令获得公钥

```bash
$ cat id_rsa.pub
```

![ssh4](https://img-blog.csdnimg.cn/img_convert/ecc4a998e7afd9aa2119a6368ec76014.png)

6. 打开`github`，点击右上角的头像，选择 `Settings`。
7. 在左侧导航栏中选择 `SSH and GPG keys`。

![ssh5](https://img-blog.csdnimg.cn/img_convert/a5338d38c720752dac94c755cebea264.png)

6. 在 `Title` 字段中，为密钥起一个描述性的名称。

   在 `Key` 字段中，粘贴你复制的公钥。
   
   ![ssh6](https://img-blog.csdnimg.cn/img_convert/5e1a086f3cad4ab1bbbeb4bd02b8b240.png)

# 补充知识

1. git中复制-->双击左键, 粘贴-->鼠标中键

3. 当Vi或Vim编辑器打开时:
   
   >(1)按下 i 键 进入插入模式，此时可以输入文本。
   >(2)输入你的提交信息。
   >(3)按下 Esc 键 退出插入模式。
   >(4)输入 :wq 并按下 Enter 键保存并退出Vi编辑器。

​		如果你想取消提交，可以按下 `Esc` 键，然后输入 `:q!` 并按下 `Enter` 键。

---
---

如有错误烦请指正。

感谢您的阅读


