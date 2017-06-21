# 2.3 查看提交历史

在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 git log 命令。

接下来的例子会用我专门用于演示的 simplegit 项目， 运行下面的命令获取该项目源代码：

`git clone git@github.com:stalloneiv/GitStudy.git`

然后在此项目中运行 git log，应该会看到下面的输出：

    commit 7af09574622d4fc2c832923cdf38c4d76baf7ed1
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:44:57 2017 +0800

        d

    commit ee9c08b1cc8ce67f70b590ce37e80c2a2a4e8ffb
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:33:09 2017 +0800

        origin/hotfix

    commit e00b96a34ad8b86a20f6a83e4bdb00a29cd041e4
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:27:56 2017 +0800

        origin/hotfix

    commit 86bd3a62eb408e5c73f4a1a4b5df18d02de27a33
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 15:49:59 2017 +0800

        origin hotfix
    :
默认不用任何参数的话，git log 会按提交时间列出所有的更新，最近的更新排在最上面。 正如你所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

git log 有许多选项可以帮助你搜寻你所要找的提交， 接下来我们介绍些最常用的。

一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交：

    commit 7af09574622d4fc2c832923cdf38c4d76baf7ed1
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:44:57 2017 +0800

        d

    diff --git a/README.md b/README.md
    index 1a96e26..b1dbb58 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,8 +1,8 @@
    -
    -# README
    -
    -### Git深度学习
    -
    -|   日期   | 版本  |  状态  |         说明         |     作者     | 审核 |
    -| -------- | ----- | ------ | -------------------- | ------------ | ---- |
    -| 20170620 | v1.00 | 未评审 |  Git深度学习初稿 | 普贤 |     |
    +^M
    +# README^M
    +^M
    +### Git深度学习^M
    :

该选项除了显示基本信息之外，还附带了每次 commit 的变化。 当进行代码审查，或者快速浏览某个搭档提交的 commit 所带来的变化的时候，这个参数就非常有用了。 你也可以为 git log 附带一系列的总结性选项。 比如说，如果你想看到每次提交的简略的统计信息，你可以使用 --stat 选项：

      commit 7af09574622d4fc2c832923cdf38c4d76baf7ed1
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:44:57 2017 +0800

        d

     README.md | 16 ++++++++--------
     1 file changed, 8 insertions(+), 8 deletions(-)

    commit ee9c08b1cc8ce67f70b590ce37e80c2a2a4e8ffb
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:33:09 2017 +0800

        origin/hotfix

     chapter2/section2.2.md | 73 +++++++++++++++++++++++++++++++++++++++++++++++---
     1 file changed, 69 insertions(+), 4 deletions(-)

    commit e00b96a34ad8b86a20f6a83e4bdb00a29cd041e4
    Author: stalloneiv <308861@qq.com>
    Date:   Tue Jun 20 18:27:56 2017 +0800

        origin/hotfix
    :

正如你所看到的，--stat 选项在每次提交的下面列出额所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

另外一个常用的选项是 --pretty。 这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 oneline 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 short，full 和 fuller 可以用，展示的信息或多或少有些不同，请自己动手实践一下看看效果如何。

        $ git log --pretty=oneline
    7af09574622d4fc2c832923cdf38c4d76baf7ed1 d
    ee9c08b1cc8ce67f70b590ce37e80c2a2a4e8ffb origin/hotfix
    e00b96a34ad8b86a20f6a83e4bdb00a29cd041e4 origin/hotfix
    86bd3a62eb408e5c73f4a1a4b5df18d02de27a33 origin hotfix
    9f6f26e6ef32a093cb24c8cd5605a92ea931e484 origin master

但最有意思的是 format，可以定制要显示的记录格式。 这样的输出对后期提取分析格外有用 — 因为你知道输出的格式不会随着 Git 的更新而发生改变：

    $ git log --pretty=format:"%h - %an, %ar : %s"
    ca82a6d - Scott Chacon, 6 years ago : changed the version number
    085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
    a11bef0 - Scott Chacon, 6 years ago : first commit

`git log --pretty=format` 常用的选项 列出了常用的格式占位符写法及其代表的意义
| 选项 |                    说明                     |
| ---- | ------------------------------------------- |
| %H   | 提交对象（commit）的完整哈希字串            |
| %h   | 提交对象的简短哈希字串                      |
| %T   | 树对象（tree）的完整哈希字串                |
| %t   | 树对象的简短哈希字串                        |
| %P   | 父对象（parent）的完整哈希字串              |
| %p   | 父对象的简短哈希字串                        |
| %an  | 作者（author）的名字                        |
| %ae  | 作者的电子邮件地址                          |
| %ad  | 作者修订日期（可以用 --date= 选项定制格式） |
| %ar  | 作者修订日期，按多久以前的方式显示          |
| %cn  | 提交者（committer）的名字                   |
| %ce  | 提交者的电子邮件地址                        |
| %cd  | 提交日期                                    |
| %cr  | 提交日期，按多久以前的方式显示              |
| %s   | 提交说明                                    |


你一定奇怪 作者 和 提交者 之间究竟有何差别， 其实作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。 所以，当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是作者，而那个核心成员就是提交者。 我们会在 分布式 Git 再详细介绍两者之间的细微差别。

当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。 这个选项添加了一些ASCII字符串来形象地展示你的分支、合并历史：

    $ git log --pretty=format:"%h %s" --graph
    * 7af0957 d
    * ee9c08b origin/hotfix
    * e00b96a origin/hotfix
    * 86bd3a6 origin hotfix
    * 9f6f26e origin master

**这种输出类型会在我们下一章学完分支与合并以后变得更加有趣。**
