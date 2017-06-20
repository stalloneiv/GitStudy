# 1.6 初次运行Git前的配置

### 初次运行 Git 前的配置
既然已经在系统上安装了 Git，你会想要做几件事来定制你的 Git 环境。
每台计算机上只需要配置一次，程序升级时会保留配置信息。
你可以在任何时候再次通过运行命令来修改它们。
Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。
这些变量存储在三个不同的位置：
1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有`--system` 选项的 git config 时，它会从此文件读写配置变量。

2. `~/.gitconfig 或 ~/.config/git/config` 文件：只针对当前用户。 可以传递 `--global` 选项让 Git 读写此文件。

3. 当前使用仓库的 Git 目录中的` config` 文件（就是 `.git/config`）：针对该仓库。

### 用户信息
当安装完 Git 应该做的第一件事就是创建SSH Key和设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：

`ssh-keygen -t rsa -C "308861@qq.com"`
`ssh -T git@github.com`   (验证ssh key是否创建成功)           
`git config --global user.name "stalloneiv"`
`git config --global user.email "308861@qq.com"`

**再次强调，如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用这些信息。**

当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。
>很多 GUI 工具都会在第一次运行时帮助你配置这些信息。

### 检查配置信息
如果想要检查你的配置，可以使用 git config --list 命令来列出所有 Git 当时能找到的配置。
`$ git config --list`
`user.name=John Doe`
`user.email=johndoe@example.com`
`color.status=auto`
`color.branch=auto`
`color.interactive=auto`
`color.diff=auto`
`...`

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：`/etc/gitconfig 与 ~/.gitconfig`）这种情况下，Git 会使用它所找到的每一个变量的最后一个配置。

你可以通过输入 git config <key>： 来检查 Git 的某一项配置
`$ git config user.name`
`John Doe`
