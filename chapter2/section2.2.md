# 2.2 记录每次本地更新并同步到远程(主机)仓库

**记录每次更新到仓库上的数据**

现在我们手上有了一个真实项目的 Git 仓库，并从这个仓库中取出了所有文件的工作拷贝。 接下来，对这些文件做些修改，在完成了一个阶段的目标之后，提交本次更新到仓库。

请记住，你工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。
1. 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。
2. 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。所以使用 Git 时文件的生命周期如下：

![enter image description here](https://git-scm.com/book/en/v2/images/lifecycle.png)

### 检查当前文件状态
要查看哪些文件处于什么状态，可以用 git status 命令。 如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

    $ git status
    On branch master
    nothing to commit, working directory clean

这说明你现在的工作目录相当干净。
换句话说，所有已跟踪文件在上次提交后都未被更改过。
此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。
最后，该命令还显示了当前所在分支，并告诉你这个分支同远程服务器上对应的分支没有偏离。
 现在，分支名是 “master”,这是默认的分支名。
 >我们在 Git 分支 会详细讨论分支和引用。

现在，让我们在项目下创建一个新的 README 文件。 如果之前并不存在这个文件，使用 git status 命令，你将看到一个新的未跟踪文件：

    echo My Project > README
    $ git status
    On branch master
    Untracked files:
    (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)

在状态报告中可以看到新建的 README 文件出现在 Untracked files 下面。
未跟踪的文件意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”， 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。
**不过现在的例子中，我们确实想要跟踪管理 README 这个文件。**
### 跟踪新文件
使用命令**git add** 开始跟踪一个文件。 所以，要跟踪 README 文件，运行：
`$ git add README`

此时再运行 git status 命令，会看到 README 文件已被跟踪，并处于暂存状态：

    $ git status
    On branch master
    Changes to be committed:

      (use "git reset HEAD <file>..." to unstage)

        new file:   README

只要在 Changes to be committed 这行下面的，就说明是已暂存状态。
如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。
 你可能会想起之前我们使用 git init 后就运行了 git add (files) 命令，开始跟踪当前目录下的文件。
 git add 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

### 暂存已修改文件
现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令，会看到下面内容：

    $ git status
    On branch master
    Changes to be committed:

      (use "git reset HEAD <file>..." to unstage)

        new file:   README

    Changes not staged for commit:

      (use "git add <file>..." to update what will be committed)

    (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

文件 CONTRIBUTING.md 出现在 Changes not staged for commit 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。
要暂存这次更新，需要运行 git add 命令。
这是个多功能命令：
- 可以用它开始跟踪新文件
- 或者把已跟踪的文件放到暂存区
- 还能用于合并时把有冲突的文件标记为已解决状态等。

**将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。**
现在让我们运行 git add 将"CONTRIBUTING.md"放到暂存区，然后再看看 git status 的输出：

    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:

      (use "git reset HEAD <file>..." to unstage)

        new file:   README

        modified:   CONTRIBUTING.md

现在两个文件都已暂存，下次提交时就会一并记录到仓库。 假设此时，你想要在 CONTRIBUTING.md 里再加条注释， 重新编辑存盘后，准备好提交。 不过且慢，再运行 git status 你会发现：现在 CONTRIBUTING.md 文件同时出现在暂存区和非暂存区。 这怎么可能呢？ 好吧，实际上 Git 只不过暂存了你运行 git add 命令时的版本， 如果你现在提交，CONTRIBUTING.md 的版本是你最后一次运行 git add 命令时的那个版本，而不是你运行 git commit 时，在工作目录中的当前版本。
 **所以，运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来：**

### 状态简览
git status 命令的输出十分详细，但其用语有些繁琐。 如果你使用 git status -s 命令或 git status --short 命令，你将得到一种更为紧凑的格式输出。 运行 git status -s ，状态报告输出如下：

    $ git status -s

     M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt
### 告诉Git哪些文件需要忽略
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。
通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。
在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。
来看一个实际的例子：
- 一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。
- 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。

在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。 来看一个实际的例子：

`$ cat .gitignore`

`*.[oa]`

`*~`
第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。
第二行告诉 Git 忽略所有以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。

此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。 要养成一开始就设置好 .gitignore 文件的习惯，以免将来误提交这类无用的文件。

文件 .gitignore 的格式规范如下：

- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。

- 可以使用标准的 glob 模式匹配。

- 匹配模式可以以（/）开头防止递归。

- 匹配模式可以以（/）结尾指定目录。

- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

### 查看已暂存和未暂存的修改
如果 git status 命令的输出对于你来说过于模糊，你想知道具体修改了什么地方，可以用 git diff 命令。 稍后我们会详细介绍 git diff.
你可能通常会用它来回答这两个问题：
- 当前做的哪些更新还没有暂存？
- 有哪些更新已经暂存起来准备好了下次提交？

尽管 git status 已经通过列出文件名的方式回答了这个问题，git diff 将通过文件补丁的格式显示具体哪些行发生了改变。

假如再次修改 README 文件后暂存，然后编辑 CONTRIBUTING.md 文件后先不暂存， 运行 status 命令将会看到：
`$ git status`

`On branch master`

`Changes to be committed:`

`(use "git reset HEAD <file>..." to unstage)`

`modified:   README`

`Changes not staged for commit:`

`(use "git add <file>..." to update what will be committed)`

`(use "git checkout -- <file>..." to discard changes in working directory)`

  `modified:   CONTRIBUTING.md`

要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 git diff：

    $ git diff
    diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
    index 8ebb991..643e24f 100644

    --- a/CONTRIBUTING.md
    +++ b/CONTRIBUTING.md
    @@ -65,7 +65,8 @@ branch directly, things can get messy.
    Please include a nice description of your changes when you submit your PR;
     if we have to read the whole diff to figure out why you're contributing
     in the first place, you're less likely to get feedback and have your change
    -merged in.
    +merged in. Also, split your changes into comprehensive chunks if your patch is
    +longer than a dozen lines.
     If you are starting to work on a particular area, feel free to submit a PR
     that highlights your work in progress (and note in the PR title that it's

### 提交更新
现在的暂存区域已经准备妥当可以提交了。
在此之前，请一定要确认还有什么修改过的或新建的文件还没有 git add 过，否则提交的时候不会记录这些还没暂存起来的变化。
这些修改过的文件只保留在本地磁盘。
所以，每次准备提交前，先用 git status 看下，是不是都已暂存起来了， 然后再运行提交命令 git commit：
`$ git commit`

这种方式会启动文本编辑器以便输入本次提交的说明。 (默认会启用 shell 的环境变量 $EDITOR 所指定的软件，一般都是 vim 或 emacs。当然也可以按照 起步 介绍的方式，使用 git config --global core.editor 命令设定你喜欢的编辑软件。）

另外，你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示：

    $ git commit -m "Story 182: Fix benchmarks for speed"
    [master 463dc4f] Story 182: Fix benchmarks for speed
      2 files changed, 2 insertions(+)
    create mode 100644 README

好，现在你已经创建了第一个提交！ 可以看到，提交后它会告诉你，当前是在哪个分支（master）提交的，本次提交的完整 SHA-1 校验和是什么（463dc4f），以及在本次提交中，有多少文件修订过，多少行添加和删改过。

**请记住，提交时记录的是放在暂存区域的快照。**
任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以很便利的回到这个状态，或者进行比较。

### 跳过使用暂存区域
尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤：

    $ git status

    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
      modified:   CONTRIBUTING.md
    no changes added to commit (use "git add" and/or "git commit -a")

    $ git commit -a -m 'added new benchmarks
    [master 83e38c7] added new benchmarks
     1 file changed, 5 insertions(+), 0 deletions(-)

### 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到：
