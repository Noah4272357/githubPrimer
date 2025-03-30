## 什么是github
为开发者提供git仓库的托管服务,是共享代码的场所,吉祥物octocat

git中,开发者将源代码存入git仓库中并加以使用,github则是在网络上提供这一项服务,也即所有源代码都用git进行管理

添加新功能后,向仓库提出申请,请求对方合并.

## pull request
开发者在本地对源代码更改后,向github中的git仓库请求合并的功能.
可以查看源代码的前后差别,还可以对指定代码进行评论.

## 社会化编程
github为软件开发者带来了民主:让所有人都有平等修改源代码的权利
程序员应该接触世界上的不同文化,扩展见闻,如果只在公司这一封闭的小世界中敲代码,不知不觉手里的技术就会变得陈腐不堪.

github与以往的仓库托管服务最大的不同在于其以人为中心,而非以项目为中心(即虽然能够知道仓库的管理者是谁,但这个管理者做了哪些事就不知道了)

## Issue
将一个任务或问题分配给一个Issue进行追踪和管理,在git提交信息中写上Issue的ID就可以自动生成从Issue到对应提交的链接

## 什么是版本管理
管理更新的历史记录,可以记录软件添加或更改源代码的过程，回滚到特定阶段，恢复误删除的文件等

版本控制系统 (VCSs) 是一类用于追踪源代码（或其他文件、文件夹）改动的工具。顾名思义，这些工具可以帮助我们管理代码的修改历史；不仅如此，它还可以让协作编码变得更方便。VCS 通过一系列的快照将某个文件夹及其内容保存了起来，每个快照都包含了顶级目录中所有的文件或文件夹的完整状态。同时它还维护了快照创建者的信息以及每个快照的相关信息等等。

版本控制系统可以回答
1. 当前模块是谁编写的，
2. 这个文件的这一行是什么时候编辑的，谁修改的，原因是什么
3. 最近的版本中什么导致了单元测试失败

版本控制系统很多，但一般的标准为git

## Fork
将某个特定仓库复制到自己的账户下


## 设置信息
git config --global user.name/user.email

用 git config --global color.ui auto可以让输出可读性更高

## 创建账户

## 设置ssh key
github上连接已有仓库的认证是通过使用ssh公开密钥的方式进行的
创建
ssh-keygen -t rsa -C "email.com"(设置保存密钥的邮箱地址)
密码可有可无
之后需要添加到github中
cat ~/.ssh/id_rsa.pub,复制内容填到github中

之后需要在github账户中填入id_rsa.pub的内容

完成后就可以用手中私人密钥和github进行认证和通信

用ssh -T git@github.com检验


## 创建目录并初始化仓库
如果时添加已有的git仓库就不用勾选，手动push即可

add .gitignore可以忽略不许管理的文件和目录
add a license 添加许可协议
大多数软件使用MIT许可协议：被授权人有权使用，复制，修改，合并，再授权和售卖软件的副本但在软件和所有副本中都必须包含版权声明和许可声明

## 开发
git clone将仓库克隆到开发环境中,将代码提交到此仓库再push到github中,代码就会公开
### git 基本操作
1. 创建目录并初始化仓库 git init
会生成.git 目录,里面存储着管理当前目录所需的仓库数据,
整个目录称为附属于仓库的工作树,文件编辑在工作树中进行,管理文件的历史快照,如果希望将文件恢复,可以从仓库中调取之前的快照
2. git status: 显示仓库状态,十分常用
3. git add filename 添加文件到暂存区,
4. git commit 创建提交,保存到仓库的历史记录中,如果需要添加详细信息,不用添加-m
提交是指记录工作树中所有文件的当前状态,对于不太需要核对的,直接用-am参数添加并提交
5. git log name可以只显示特定目录或文件的修改记录,-p可以显示文件的前后差别
简洁版用 --pretty=short,加目录或文件名可以只显示对应修改

6. git diff 查看工作树与暂存区的差别
git diff <filename>:显示与暂存区文件的差异
git diff <revision><filename>:显示某文件两版本之间差异

如果用add添加后,用HEAD查看和最新提交的差别,注意,最好在git commit之前用这个命令查看一下差别,确认完毕再提交

git help <command>:获取帮助

### 分支
并行开发时使用.现在软件开发一般会保留稳定分支,同时开发多个特性分支实现新功能.
主干分支则是合并的原点.为在历史记录中记录下分支合并,需要创建合并提交,在merge中用
--no-ff参数;git log --all --graph --decorate:可视化合并历史记录
git checkout <revision>更新HEAD和目前分支,用-表示上一分支
git branch:显示分支
git branch <name>创建分支,此后执行的命令仅存在于该分支,不同之间互不影响
git merge <revision>合并到当前分支
git mergetool:使用工具处理合并冲突

git log只能查看以当前状态为终点的历史日志.用git reflog可以查看仓库的操作日志.
找到回溯历史的哈希值,用git reset --hard恢复到历史前的状态.

git commit --amend: 编辑提交的内容或信息
git reset HEAD <file>:恢复暂存的文件
git checkout --<file>:丢弃修改

如果提交的内容中有拼写错误,需要提交修改,将修改包含到前一个提交中,压缩成一个历史记录

### 推送远程
需要远程创建并与本地名保持一致.
git remote：列出远端
git remote add<name> <url>添加一个远端,将其设置为本地仓库的远程仓库
git push <remote> <local branch>:<remote branch>: 将对象传送至远端并更新远端引用,-u origin master参数可以在推送的同时将origin仓库的master分支设置为本地仓库当前分支的上游,从而在用gitpull从远程仓库获取内容时可以直接从origin的master分支获取,不用额外加参数.当然也可以推送其他分支.

git checkout -b test origin/test,从仓库获取其他分支到本地分支,之后用git push就是提交该分支

git pull origin test 将本地的分支更新到最新状态
如果修改同一份源代码,push很容易冲突最好频繁push pull
git clone:从远端下载仓库,默认处于master分支下

### 进一步学习
progit;learngitbranching;trygit

# git数据模型
## 快照
git将顶级目录中的文件和文件夹称为集合，并通过一系列快照管理历史记录，git中文件被称作blob对象，即数据，目录则被称为树，快照则是被最总的最顶层的树

## 关联快照
git中，历史记录是一个由快照组成的有向无环图（每个快照都有一系列父辈）
快照被称为提交

## git数据模型
// 文件即数据
type blob = array<byte>

// 包含文件和目录的目录
type tree = map<string,tree| blob>

// 每次提交都包含父辈，数据和顶层树
type commit =sturct{
    parents: array<commit>
    author: string
    message: string
    snapshot: tree
}

git中对象可以为上述三者之一

git储存数据时，所有对象都会基于它们的哈希值进行寻址

objects=map<string,object>

def sotre(object):
    id=sha1(object)
    objects[id]=object

def load(id):
    return objects[id]

当对象在引用其他对象时，没有在硬盘上保存这些对象，而是仅仅保存了哈希值作为引用

但用哈希值标记快照的方式不方便，git的解决方法是给哈希值 references,引用是指向提交的指针，但可变
例如master通常会指向主分支最新一次提交

references = map<string,string>

def update_reference(name,id):
    references[name]=id

def read_reference(name):
    return references[name]

def load_reference(name_or_id):
    if name_or_id in references:
    return load(references[name_or_id])
    else:
    return load(name_or_id)

通常情况下，我们会想要指导我们当前所在的位置，并将其标记下来，这样我们创建新的快照的时候，就可以知道其相对位置，git中，当前位置的索引为HEAD

# 仓库
即对象和引用，硬盘上，git仅存储对象和引用,所有git命令都对应着提交树的操作，例如增加对象，增删引用

当输入指令时，可以思考该命令是如何对底层的数据结构进行操作的

# 暂存区
staging area , 允许指定下次快照中要包括的那些改动








# git 
git 是分散型版本管理系统

注释：集中型与分散型的不同，集中型将仓库集中存放在服务器中，所以只存在一个仓库，
但一但开发者的环境不能连接服务器，就无法获取最新的源代码，开发就无法进行。

分散型将仓库fork给每一个用户，fork出的仓库和原仓库是两个不同的仓库，在通过push和pull更改仓库




## 分支
git checkout -b name 创建分支并切换到该分支
git checkout name 切换, -表示上一分支
此时提交只会提交到当前分支
git merge 合并分支 --no-ff name
可以用git log --graph查看topic分支的内容





### 1. **Clone the Repository Locally**

First, you need to clone the GitHub repository to your local machine:

- **Using the Command Line:**
  1. Open your terminal or command prompt.
  2. Navigate to the directory where you want to clone the repository.
  3. Use the `git clone` command followed by the repository URL:
     ```bash
     git clone https://github.com/username/repository.git
     ```
  4. Replace `username` and `repository` with the appropriate GitHub username and repository name.

- **Using VSCode:**
  1. Open VSCode.
  2. Press `Ctrl + Shift + P` (or `Cmd + Shift + P` on Mac) to open the Command Palette.
  3. Type and select **Git: Clone**.
  4. Enter the URL of the repository and select a local folder to clone it to.

### 2. **Open the Cloned Repository in VSCode**

After cloning, you can open the repository:

- **If you used the command line:**
  1. Navigate to the cloned repository folder using the terminal.
  2. Run the command:
     ```bash
     code .
     ```
  This command opens the current directory in VSCode.

- **If you used VSCode to clone:**
  1. You should already be in the repository. If you closed VSCode, you can open it again and use **File > Open Folder...** to navigate to the cloned repository folder.

### 3. **Install the GitHub Pull Requests and Issues Extension (Optional)**

For enhanced GitHub integration:

1. Go to the Extensions view in VSCode by clicking on the Extensions icon in the Activity Bar on the side.
2. Search for "GitHub Pull Requests and Issues" and install it.

This extension allows you to manage pull requests and issues directly within VSCode.

### 4. **Making Changes and Committing**

After opening the repository, you can make changes:

1. Make your edits in the code files.
2. Save your changes.
3. Use the Source Control view (click the Source Control icon in the Activity Bar) to stage and commit your changes.

### 5. **Pushing Changes Back to GitHub**

When you're ready to push your changes:

1. In the Source Control view, click the "..." (More Actions) icon.
2. Select **Push** to upload your local commits to the remote repository on GitHub.

### Additional Tips

- **Open a Remote Repository Directly**: If you want to open a repository directly from GitHub without cloning (especially useful for quick edits), you can use the GitHub Codespaces feature if it’s available for that repository.
- **VSCode Settings**: Make sure you have Git installed and properly configured in VSCode settings for a seamless experience.

If you have any specific questions or need clarification on any steps, feel free to ask!

Notice: 用git clone时关闭vpn,否则可能需要指定代理窗口