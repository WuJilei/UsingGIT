GIT Study Notes
===
[learn from Liao Xuefeng](https://www.liaoxuefeng.com/)

# GIT简介
## 安装GIT
### 在Mac Os X上安装GIT
1、安装homebrew， 通过homebrew安装GIT，参考：[homebrew文档](http://brew.sh/)
2、从Appstore中安装Xcode，运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads"，选择”Command Line Tools“，点“Install”就可以完成安装
![安装Xcode](https://www.liaoxuefeng.com/files/attachments/001384907061183ba2a452af9de4a8a8640339239bc3e5e000/0)

### 在Windows上安装GIT
借用模拟环境和GIT打包好的程序msysgit，单独下载一个可执行文件即可，下载路径https:://git-for-windows.github.io下载，或者[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)，然后按照默认选项安装。安装完毕后，在开始菜单中找到“GIT”->"GIT Bash"，弹出命令窗口，说明安装成功，接下来完成最后一步设置：
```
$git config -- global user.name "Your Name"
$git config -- global user.email "email@example.com"
```
![windows setup](https://www.liaoxuefeng.com/files/attachments/001384907073134ef6feff559cf4ce3a2c5c588d2831c0a000/0)

因为GIT是分布式版本控制系统，所以每个机器都必须自爆家门，即名字和email地址。注意，**git config** 命令的**--global** 参数，用了这个参数，表示这台机器上所有的git仓库都会使用这个配置，也可以对某个仓库指定使用不同的用户名和email地址。

## 创建版本库
版本库（repository）可理解为目录，目录中的所有文件都可以被GIT管理起来，包括修改、删除等被git跟踪，创建步骤如下：
1、选择一个合适位置，创建一个空目录：
```
$mkdir learngit
$cd learngit
$pwd
$/Users/michael/learngit
```
**pwd**命令用于显示当前目录，在示例中，仓库位于/Users/michael/learngit。
2、通过**git init**命令把这个目录变成git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
瞬间git就把仓库健康了，并且告诉你是一个空的仓库（empty Git repository），若仔细看当前目录下多出一个**.git**的目录，这个目录是git用来跟踪管理版本库的，这个目录的文件不要动，若没有看到，则可以通过 **ls -ah**命令可以查看。
注：也不一定必须在空的目录下创建GIT仓库，已经有东西的目录也可以，但注意副作用。

注：所有的版本控制系统，只能跟踪文本文件的改动，包括txt文件、网页和程序代码等，因此microsoft的word格式是二进制格式，版本控制系统无法跟踪word文档的变动情况。
另外，windows自带的note记事本程序编辑的文本文件也涉及到文件头问题，建议文本文件的编辑使用[Notepad++](http://notepad-plus-plus.org/)来代替，把NotePad++的默认编码设置为UTF-8 without BOM即可。
！[link](https://www.liaoxuefeng.com/files/attachments/001384907170801199e153159cc4a438bed8d255edf157a000/0)

示例：我们先编辑一个**readme.txt**文件，内容如下：
```
Git is a version control system.
Git is free software.
```
一定要放到**learngit**目录下（子目录也可），因为这里是一个git仓库，放到其他位置则找不到这个文件。接下来把新建的文件添加到git仓库中。

第一步：用命令 **git add**告诉Git，把文件提交到仓库：
```
$git add readme.txt
```
执行上面的名利，没有任何显示——Unix的哲学是“没有消息就是好消息”，说明添加成功。
第二步：用命令 **git commit** 告诉Git，把文件提交到仓库：
```
$git commit -m "wrote a readme file"
[master (root-commit) cb926e7] wrote a readme file
1 file changed, 2 insertions (+)
create mode 100644 readme.txt
```
注： **git commit**命令，**-m** 后面输入的本次提交的说明，可以输入任何内容，最好是有意义的信息以便于后续在历史记录中查找； 不想输入 **-m "xxx"** 也可以，可以查阅如何处理；**git commit** 命令执行成功后会告诉用户，1个文件被改动，插入了两行内容（即readme.txt内的两行内容）。

添加文件需要**add**, 和 **commit** 两步，因为**commit** 可以一次提交很多次 **add**的不同文件，如：
```
 $ git add file1. txt
$git add file2.txt file3.txt
$git commit -m "add 3 files."
```
# 时光机穿梭

上述内容已经完成增加一个readme.txt文件的功能，接下来我们继续修改readme.txt文件的内容，如：
```
Git is a distributed version control system.
Git is free software.
```
现在，运行**git status** 命令查看结果：
```
$ git status 
```
显示略

**git status** 命令可以让我们时刻掌握仓库的状态，上述命令会显示readme.txt已经被修改过了，但没有准备提交的修改。

虽然Git告诉我们readme.txt被修改了，但想看看具体修改了什么内容，自然是很好的，这里需要**git diff**命令查看：
```
$git diff readme.txt
```
**git diff** 顾名思义就是查看difference，显示格式为Unix通用的diff格式，即可以看出第一行中增加了“distributed”单词。知道了对readme.txt做了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是 **git add**
```
$git add readme.txt
```
应该没有任何输出，在执行第二步前，我们在用**git status**查看仓库状态：
```
$git status
```
**git status** 告诉我们，将要被提交的修改包括readme.txt文件，下一步可以放心的提交了：
```
$git commit -m "add distributed"
```
提交后，我们再用**git status** 查看仓库当前状态，git将告诉我们当前没有需要提交的修改，且工作目录是干净的。

### 版本退回
上述我们学会了修改文件，并将修改版本提交到Git版本库，现在可以再练习一次，修改readme.txt文件如下：
```
Git is a distributed version control system.
Git is free software distributed under the GPL
```
然后尝试提交：
```
$git add readme.txt
$git commit -m"append GPL"
```
类似，我们可以不断的对文件进行修改，然后不断提交到版本库中，每当你觉得文件修改到一定程度的时候，就可以保存一个快照，这个快照在Git中被称之为**commit**，一旦把文件改乱了，或者误删了文件，还可以从最近的一个**commit**中恢复，然后继续进行工作，而不是把几个月的工作成果全部丢失。现在，我们可以查看readme.txt一共有多少个版本被提交到git仓库里了，使用命令：**git log**
```
$git log
```
**git log ** 命令显示从最近到最远的提交日志，我们看到有3次提交，最近一次是**append GPL**， 上一次是**add distributed**, 最早一次是**wrote a readme file**. 如感觉输出信息太多，则可以增加一个参数： **--pretty=oneline**参数：
```
$git log --pretty=oneline1
```
tips: 上述命令执行后有一大串字符是**commit id**（版本号），和SVN不一样，Git的**commit id**不是1、2、3...等递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且每台计算机显示可能不同，因为Git作为分布式版本控制系统，不可能用1、2、3...等作为版本号。
每提交一个新版本，实际上Git就会把他们自动串成一条时间线，若用可视化工具可以更清楚的看到提交的历史时间线。

现在，我们启动时光穿梭机，准备把readme.txt退回到上一个版本，也就是**add distributed**的那个版本。
1、首先，Git必须知道当前版本是哪个，在Git中， 用**HEAD**表示当前版本，也就是最新提交的，上一个版本就是**HEAD^**，上上个版本就是**HEAD^^**，当然，往上100个版本撰写**^**比较容易混乱，可以写成**HEAD~100**。现在我们把当前版本**append GPL**回退到上一个版本**add distributed**，就可以使用**git reset**命令：
```
$git reset --hard HEAD^
```
**--hard**的参数含义后续阐述，这里再查看readme.txt的内容是不是**add distributed**内容：
```
$ cat readme.txt
```
当然显示为我们预计的内容。这里还可以继续回退到上一个版本**wrote a readme file**,不过这里我们先查看一下版本库的状态：**git log**
```
$git log
```
我们发现最新的那个版本**append GPL**已经看不到了，若我们反悔想回去，如何处理？这时候若命令窗口还没有关掉，则我们可以查找原来**append GPL**的**commit id**是**2d617...**，则我们可以指定这个id号码回到未来的某个版本：
```
$git reset --hard 2d617
```
注：版本号没有必要写全，Git会自动去找，但若仅写前1~2位，则Git可能会找到多个版本号，就无法确认是哪个了。

Git的版本回退速度非常快，因为Git内部有个指向当前版本的**HEAD**指针，当做回退处理时，Git仅仅是把HEAD从指向**append GPL**变为**add distributed**，顺便更新工作区的文件，所以若让**HEAD**指向那个文件，就把当前版本定位到哪里。

但，若我们回退到了一个版本，关闭了系统，结果第二天反悔了，想恢复到新版本如何处理？这时找不到新版本的**commit id**,注意，在Git中，当使用**$git reset --hard HEAD^**回退到**add distributed**版本时，想再恢复到**append GPL**，就必须找到**append GPL**的**commit id**, Git提供了一个命令： **git reflog**用来记录每一次命令。
```
$git reflog
```
上述命令中，我们可以看到第三行中就显示了**append GPL**的*commit id*是**2d617d6**，那么我们又可以参照前述命令乘坐时光机回到未来了。

### 工作区和暂存区
Git和其他版本控制系统诸如SVN的不同之处在于有暂存区的概念。
#### 工作区（Working Directory）
即在电脑中的目录，即GIT系统指定的本地工作文件夹。

#### 版本库（Repository）
工作区有一个隐藏目录**.git**，这个不算工作区而是Git的版本库。版本库中存储很多内容，其中最重要的是stage（或者index）的暂存区，还有Git为我们自动创建的第一个分支**master**以及指向**master**的一个指针**HEAD**.
我们把文件往Git版本库中添加的时候，使用两个步骤进行的：
第一步使用**git add**把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步使用**git commit**提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个**master**分支，所以现在的**git commit**就是往**master**分支上提交更改。或者简单的理解为需要提交的文件修改通通放到暂存区，然后一次性提交暂存区的所有修改。实际练习如下：
1、先对readme.txt文件进行修改，比如增加一行内容：*Git has a mutable index called stage.*
2、然后在工作区中新增加个**LICENSE**文本文件（内容随意）。
3、先用**git status**查看一下状态：
```
$git status
```
上述命令结果会清楚的告诉我们：**readme.txt**被修改了，而**LICENSE**尚未被添加过，所以它的状态是**Untracked**。现在使用两次**git add**命令吧readme.txt和**LICENSE**都添加，再用**git status**查看一下：
```
$git status
```
现在，工作区中有2个文件，**git add**实际是把需要提交的修改放到暂存区(Stage)，然后我们执行**git commit**命令就可以一次性把暂存区的所有修改提交到分支。
```
$git commit -m "understand how stage works by add two file "
```
一旦提交后，若没有对工作区做任何修改，则工作区是“干净”的。
```
$git status
```
注：这里理解暂存区是Git非常重要的概念，明白了暂存区，就能搞清楚Git的很多操作。

### 管理修改
Git系统比其它版本控制系统设计的优秀在于Git是跟踪并管理的是修改。修改包括删除、增加一行，修改字符和删除字符，甚至创建一个新文件也算一个修改。试验如下：
1、对readme.txt文件修改，增加一行内容： *Git tracks changes.*
2、然后添加：
```
$git add readme.txt
$git status
```
3、然后再修改readme.txt，修改最后一行：*Git tracks changes of files.* 提交并查看状态：
```
$git commit -m "git tracks changes"
$ git status
```
这里将发现，第二次修改没有被提交？因为我们的操作过程为： 第一次修改-> **git add** -> 第二次修改 -> **git commit**, 因为Git管理的是修改，所以当用**git add**命令后，在工作区的第一次修改被放入暂存区，准备提交，但是在工作区的第二次修改并没有放到暂存区，所以**git commit**只负责把暂存区的修改提交了，也就是第一次修改被提交了，但第二次修改不会被提交。 提交后，可以用 **git diff HEAD --readme.txt** 命令查看工作区和版本库里面最新版本的区别：
```
$git diif HEAD --readme.txt
```
可以发现，第二次修改确实没有被提交。若要提交第二次修改，则可以继续**git add**，然后再**git commit** 或者第一次修改后咱补提交，先**git add**第二次修改，然后再**git commit**，相当于把两次修改一次性提交。

### 撤销修改
一般情况下，我们的工作可能会出现错误，如在文本中增加了一些不合适的词语，若这些错误是发生在准备提交之前，则可以直接修改文本文件，手动把文件恢复成上一个版本的状况，这时候若用： **git status**检查，结果会发现：
```
no changes add to commit (use "git add" and/or "git commit -a")
```
或者利用git的命令： **git checkout --file** 丢弃工作区的修改。
```
$ git checkout --readme.txt
```
上述命令**git checkout --readme.txt**的意思就是，把**readme.txt**文件在工作区的修改全部撤销，这里有两种情况：
1、一种是**readme.txt**自修改后还没有放到暂存区，现在撤销修改就回到和版本库一模一样的状态；
2、一种是**readme.txt**已经添加到暂存区后，又做了修改，那么现在的撤销就回到添加到暂存区后的状态。
总之，**git checkout** 就是让这个文件回到最近一次**git commit**或**git add**时的状态。

注：**git checkout --file**命令中的**--** 非常重要，没有**--**，就变成了”切换到另一个分支“的命令了，后面分支管理中会再次分析**git checkout**命令。

若出错的文件已经通过**git add** 转到暂存区了，则若没有**commit**，比如用**git status**查看了一下，发现修改只是添加了暂存区，尚未提交，则可以用命令： **git reset HEAD file**可以把暂存区的修改撤销掉（unstage），重新放回工作区。
```
$git reset HEAD readme.txt
```
即：**git reset** 命令既可以回退版本，也可以把暂存区的修改退回到工作区，当我们用**HEAD**时，表示最新的版本。
这时候在用**git status**查看，就可以看到暂存区也是干净的，工作区有修改，而工作区的修改可以用**git checkout --filename.txt**进行放弃。

**注意**，若修改错误的内容，已经从暂存区提交到了版本库，那么只能通过版本回退的办法退回到上一个版本，不过这里是有条件的，即还没有把当前本地版本推送到 远程！若已经推送到远程，因为Git是分布式版本控制系统，那末就回天无力了！！！
总结：1、仅修改乱了工作区的文件内容，可直接放弃工作区的修改，使用命令：**git checkout --file**；
2、若修改了工作区的文件内容，且通过**git add file**命令添加到暂存区，想放弃修改，则需要两个步骤，其一：**git reset HEAD file**将暂存区的文件退回到工作区；其二，放弃工作区的修改（即第1种情况）；
3、若已经提交了不合适的文件到了版本库，若想撤销本次提交，需要选择版本回退功能，但这里有前提是：没有推送到远程库！！！

### 删除文件
在Git中，删除也是一个修改操作，比如先添加一个新文件test.txt到Git并提交：
```
$git add test.txt
$git commit -m "add test.txt"
```
一般情况下，我们是通过文件管理器将无用的文件删除，或者Mac系统中用**rm** 命令删除：
```
$ rm test.txt
```
那么这时候Git应该知道有文件删除了，用**git status**命令既可以告诉那些文件已经被删除了；
```
$git status
```
那么，现在有两个选择：1、确实要从版本库中删除该文件，则可以通过命令**git rm**和**git commit**确认：
```
$git rm test.txt
$git commit -m "remove test.txt"
```
以上命令确保文件从版本库中删除了；
另一种情况就是错删了，这时候我们需要恢复，因为版本库中还有呢，所以可以很轻松的将误删除的文件恢复到最新版本：
```
$git checkout --test.txt
```
**git checkout**是用版本库的版本替换工作区的版本，无论工作区的修改还是删除，都可以”一键还原“！

注：命令**git rm**用于删除一个文件，若一个文件已经被提交到版本库，则永远不用担心误删，但要注意，只能恢复到文件的最新版本，有可能会丢失最近一次提交后你修改的内容。

# 远程仓库

本地版本管理的Git功能和SVN差异不大，但重点是Git具备远程仓库等功能——Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上，即从最早的一台计算机从原始版本库，之后别的机器可以”克隆“这个原始版本库，从而每台计算机的版本变为一样，没有主次之分。当然，一台电脑上也可以克隆多个版本库，主要不在同一个目录下，不过现实中一台计算机上多个远程库没有意义，硬盘一旦出了故障则所有的库就会挂掉。因此，实际情况是找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个服务器仓库克隆一份到自己的电脑上，并且把各自把各自的提交推送到服务器仓库中，也从服务器仓库中拉取别人的提交。

当然，一方面可以自己构建一台运行Git的服务器，也可以借用[GitHub](https://github.com/)的网站，这个网站提供Git仓库托管服务，只要注册一个GitHub帐号，就可以免费得到Git远程仓库服务。

接下来的工作就是注册GitHub账号，然后将本地Git仓库和GitHub仓库之间的传输建立起来，注意这里的传输是通过SSH加密的，因此需要一点设置：
1、创建SSH Key。在用户主目录下，若没有.ssh目录，如有，则查看目录下是否有**id_rsa**和**id_rsa.pub**这两个文件，若存在，则可以直接下一步；若没有，则需要贷款Shell（Windows下打开Git Bash），创建SSH Key：
```
ssh-keygen -t rsa -C "your email@example.com"
```
注意这里的邮件地址更换为自己的邮件地址，然后一路回车，使用默认值即可，由于这个key不用与军事等加密目标，故也无需设置密码。若一切顺利的话，在用户主目录中可以查到**.ssh**目录，里面有**id_rsa**和**id_rsa.pub**两个文件，这两个就是SSH Key的密钥对， **id_rsa**是私钥，不能泄露；**.id_rsa.pub**是公钥，可以放心告诉任何人。
2、登录GitHub，打开"Account settings"，"SSH Keys"页面，然后，点击"Add SSH Key"，填上任意Title，在Key文本框里粘贴**id_rsa.pub**文件的内容。 点击"Add Key"，这时候就应该看到已经添加的Key。

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的内容确实为你推送的，而不是别人冒充的，Git支持SSH协议，所以只要GitHub知道了你的公钥，就可以确认只有你自己才能推送。当然，GitHub允许添加多个Key，特别有若干电脑，一会你可以在公司提交，一会儿可以在家提交，只要把每台计算机的Key都添加到GitHub中，就可以在每台电脑上向GitHub推送内容了。

注：在GitHub上免费托管的Git仓库，任何人都可以看到（只有你自己才能修改），所以不要将敏感内容放进去。若不想让别人看到你的Git库，有两个办法，一是提交保护费，让GitHub把公开的仓库变为私有，这样别人就看不见了（也不可以读和写），另一个办法就是搭建Git服务器。

### 添加远程库
若在本地创建了一个Git仓库，又想GitHub上创建一个Git仓库，并且让这两个仓库远程同步，这样GitHub的仓库即可以作为备份，又可以让其他人通过该仓库来协作，一举多得。
首先登录到GitHub，然后在右上角找到**Create a new repo** 按钮，创建一个新仓库；
在Repository name中填入**learnGit**，其他保持默认值，点击**Create repository**按钮，就成功创建了一个新的Git仓库。目前GitHub上的这个**learnGit**仓库还是空的，GitHub告诉我们可以从这个仓库中克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到GitHub仓库。

现在，依据GitHub的提示，在本地**learnGit**仓库中运行命令：
```
$ git remote add origin git@github.com:michaelliao/learngit.git
```
千万注意，上面的**michaelliao**替换成自己的GitHub账户名，否则你在本地关联的是我的远程库，关联没有问题，但无法远程推送内容，因为SSH Key不在我的GitHub网站的账户列表中。

添加后，远程库的名字就是**origin**，这个是Git默认的叫法，也可以改成别的，但Origin这个名字一看就知道是远程库。下一步就是吧本地库的所有内容推动到远程库上：
```
$git push -u origin master
counting objects: 19, done.
Delta compression using up to 4 threads.
Compressing objects: 100 % (19/）， done。 
Writing objects : 100% (19/19),13.73 KiB, done.
Total 23 (delta 6), reusewd 0 (delta 0) 
TO git@github.com:michelliao/learngit.git
  *(new branch)  Master-Master
Branch master set up to track remote branch master from origin.
```
把本地库的内容推送到远程，可以用**git push**命令，实际上是把当前分支**master**推送到远程。由于远程库是空的，我们第一次推送**master**分支时，增加了**-u**参数，Git不但会把本地的**master**分支内容推送到远程新的**master**分支，还会把本地的**master**分支和远程的**master**分支关联起来，在以后的推送或者拉取时就可以简化名利。推送成功后，就可立刻在GitHub在页面中看到远程库的内容已经和本地一模一样了。从现在气，只要本地做了提交，就可以通过命令：
```
git push origin master
```
把本地**master**分支的最新修改推送到GitHub，现在我们就拥有了真正的分布式版本库。

####SSH警告
当第一次使用Git的**clone**或**push**命令连接GitHub时，会得到一个警告：
```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的key时，需要确认GitHub的key的指纹信息是否真的来自Github的服务器，输入**yes**回车即可。Git会输出一个警告，告诉你已经把GitHub的key添加到本机的一个信任列表里了：
```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```
这个警告只会出现一次，后面操作就不会有任何警告了。若是咋担心有人冒充GitHub服务器，输入**yes**之前可以对照GitHub的RSA Key指纹信息是否与SSH连接给出的一致。

###  从远程库克隆
若从零开发，那么最好的方式是先创建远程库，然后从远程库克隆。首先登录GGItHub，创建一个新的仓库，名字如叫做：**gitskills** ：注意，勾选中 **Initialize this repository with a README**, 这样GitHub会自动创建一个**README.md**文件，创建完毕后就可以查看这个文件。下一步就是用命令**git clone**克隆一个本地库：
```
 $ git clone git@github.com:michaelliao/gitsskills.git
Cloing into 'gitskillss'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

 $ cd gitskills
$ ls
README.md
```
注意把Git库的地址换成你自己的，然后进入**gitskills** 目录看看，已经有**README.md**文件了。若有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

注：GitHub给出的地址不止一个，还可以用 **https://githum.com/michaelliao/gitskills.git** 这样的地址。实际上，Git支持多种协议，默认的**git://**使用ssh，但也可以使用**https**等其他协议。使用**https**除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但在某些只开发http端口的公司内部就无法使用**ssh**协议而只能用**https**。

# 分支管理

### 创建与合并分支

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截至到现在，只有一条时间线，在Git中这个分支叫主分支，即**master** 分支，**HEAD**严格来说不是指向提交，而是指向**master**，**master**才是指向提交的，所以**HEAD**指向的就是当前分支。一开始的时候，**master**分支是一条线，Git用**master**指向最新的提交，再用**HEAD**指向**master**，就能确定当前分支，以及当前分支的提交点，每次提交，**master**分支都会向前移动一步，这样，随着不断提交，**master**分支的线也越来越长。当创建新的分支，例如**dev**时，Git新建了一个指针叫**dev**，指向**master**相同的提交，再把**HEAD**指向**dev**，就表示当前分支在**dev**上。

Git创建分支很快，因为除了增加一个**dev**指针，改变**HEAD**的指向，工作区的文件都没有任何变化！不过，从现在开始，对工作区的修改和提交就是针对**dev**分支了，比如新提交一次后，**dev**指针往前移动一步，而**master**指针不变。若我们在**dev**上的工作完成了，就可以把**dev**合并到**master**上，做简单的方法，直接把**master**指向**dev**的当前提交，就完成了合并，所以Git合并分支也很快，更改指针而工作区内容不变。

合并完分支后，甚至可以删除**dev**分支，删除**dev**分支就是把**dev**指针给删掉，最后只剩下一条**master**分支。以下为实际练习：
首先，创建**dev**分支，然后切换到**dev**分支：
```
 $git checkout -b dev
Switched to a new branch 'dev'
```
**git checkout**命令加上**-b**参数表示创建切换，相当于以下两条命令：
```
 $git branch dev
 $git checkout dev
Switched to branch 'dev'
```
然后，用**git branch**命令查看当前分支：
```
 $git branch
* dev 
  master
```
**git branch**命令会列出所有分支，当前分支前会标有** * **号。然后，我们就可以在**dev**分支上正常提交，比如对readme.txt做一个修改，加入一行：
```
 Creating a new branch is quick
```
然后提交：
```
 $ git add readme.txt
 $ git commit -m "branch test"
[dev fec145a] branch test
 1 file changed,  1 insertion(+)
```
现在，**dev**分支的工作完成，我们可以切换回**master**分支：
```
 $ git checkout master
Switched to branch 'master'
```
切换回**master**分支后，在查看一个readme.txt文件，刚才添加的内容不见了，因为那个提交是在**dev**分支上，而**master**分支此刻的提交点并没有变。现在，我们把**dev**分支的工作成果合并到**master**分支上：
```
 $ git merge dev
Updating d17efd8..fec145a
Fast-forward
readme.txt | 1 +
1 file changed, 1 insertion (+)
```
**git merge** 命令用于合并指定分支到当前分支，合并后再查看readme.txt的内容，就可以看到和**dev**分支的最新提交是完全一样的。注意到上面的**Fast-forward**信息，Git告诉这次合并是"快进模式"，也就是直接把**master**指向**dev**的当前提交，所以合并速度非常快，当然，也不是每次合并都能**Fast-forward**，合并完成后，就可以放心删除**dev**分支了：
```
 $ git branch -d dev
Deleted branch dev (was fec145a)
```
删除后，再查看**branch**，就只剩下**master**分支了：
```
 $ git branch 
* master
```
因为创建、合并和删除分支非常快，所以Git鼓励使用分支完成某项任务，合并后再删除分支，这个和直接在**master**分支上工作效果是一样的，但过程更安全。

### 解决冲突

合并分支过程中，有可能出现冲突，如我们准备一个新的分支**feature1**分支，继续我们的新分支开发：
```
 $ git checkout -b feature1
Switched to a new branch 'feature1'
```
修改readme.txt最后一行，改为：
```
 Creating a new branch is quick AND simple.
```
在**feature1**分支上提交：
```
 $ git add readme.txt
 $ git commit -m "AND simple" 
[feature1 75a857c] AND simple 
1 file changed, 1 insertion (+), 1 deletion(-)
```
切换到**master**分支：
```
 $ git checkout master
Switched to branch 'master'
YOur branch is ahead of 'origin/master' by 1 commit.
```
Git还会自动提示我们当前**master**分支比远程的**master**分支要超前1个提交。在**master**分支上把readme.txt文件的最后一行改为：
```
 Creating a new branch is quick & simple.
```
提交： 
```
 $ git add readme.txt
 $ git commit -m "& simple"
[master 400b400] & simple
1 file changed, 1 insertion (+), 1 deletion (-)
```
现在**master**分支和**feature1**分支各自都分别有新的提交，这种情况下，Git无法执行快速合并，只能试图把各自的修改合并起来，但这种合并就会发生冲突，如：
```
 $ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Mearge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
果然冲突了，Git告诉readme.txt文件存在冲突，必须手动解决冲突后再提交，**git status**也可以告诉我们冲突文件的具体情况，如：
```
 $ git status
```
我们可以直接查看readme.txt的内容：
```
 Git is a distrubted version congtrol system.
 Git is free software distributed under the  GPL.
 Git has a mutable index called stage. 
 Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
 =======
Creating a new branch is quick AND simple.
>>>>>>> feature 1
```
Git用**<<<<<<<<**，**=======**，**>>>>>>>**标记出不同分支的内容，我们修改如下后保存：
```
 Creating a new branch is quick and simple. 
```
再提交： 
```
 $ git add readme.txt
 $ git commit -m "conflict fixed"
[master 59bc1ch] conflict fixed
```
现在，**master**分支和**feature1**分支就变成了合并后的的结果，可以用带参数的**git log**显示分支的合并情况：
```
 $ git log --graph -- pretty=oneline  --abbrev- commit
* 59bc1cb conflict fixed 
|\
| * 75a857c AND simple 
* | 400b400 & simple
|/
* fec145a branch test 
...
```
最后删除** feature1** 分支，
```
 $git branch -d feature1 
Deleted branch feature1 （was 75a857c).
```
工作完成。

### 分支管理策略
通常合并分支时，如有可能，Git会用**Fast forward**模式，但在这种模式下，删除分支会丢掉分支信息，若要强制禁用**Fast forward**模式，Git就会在merge时生成一个新的commit，这样从分支历史上就可以看出分支信息，下面是实践练习，利用**--no-ff**方式的**git merge**， 首先仍然创建并切换**dev**分支：
```
 $git checkout -b dev
Switched to a new branch 'dev'
```
修改readme.txt文件，并提交一个新的commit：
```
 $ git add readme.txt
 $ git commit -m "add merge"
[dev 6224937] add merge
1 file changed, 1 insertion (+)
```
现在，我们切换回**master**, 准备合并**dev**分支，请注意**--no-ff**参数，表示禁用**Fast forward**:
```
 $ git checkout master
Switched to branch 'master'
 $ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
readme.txt | 1+
1 file changed, 1 insertion(+)
```
因为本次合并要创建一个新的commit，所以加上**-m**参数，把commit描述写进去。合并后，可以用**git log**查看分支历史。

#### 分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：首先，**master** 分支应该是非常稳定的，也就是仅仅用了发布新版本，平时不能在上面干活；干活一般在**dev**分支上，也就是说**dev**分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；所以团队合作中，每个人都在dev分支上干活，每个人都有自己的分支，时不时地往**dev**分支上合并就可以了。

### Bug分支
软件开发中，bug就像家常便饭，有bug就需要修复，在Git中，由于分支如此强大，每个bug都可以通过一个新的临时分支来修复，修复后合并分支，然后将临时分支删除。当接到一个修复一个代号101的bug的任务时，很自然就会创建一个分支**issue-101**来修复它，但若当前正在**dev**上的工作没有提交时：
```
 $ git status
```
这时并不是不想提交，而是工作只进行了一半， 还没法提交，预计完成还需要1天时间，但必须在两个小时内修复该bug，如何处理？这时就需要采用Git提供的**stash**功能，把当前的工作现场『储藏』起来，等以后恢复现场后继续工作。
```
 $ git stash
Saved working directory and index stat WIP on dev: 6224937 add merge
HEAD is now at 6224937 add merge
```
现在用**git status** 查看工作区，这是是干净的（除非有没有被Git管理的文件），此时可以放心地创建分支来修复bug。首先确定要在哪个分支上修复bug，假定需要在**master**分支上修复，就从**master**创建临时分支：
```
 $ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
 $ git checkout -b issue-101
Switched to a new branch 'issue-101'
```
现在修复bug，需要吧『Git is free software...』 改为『Git is a free software...』,然后提交：
```
 $ git add readme.txt
 $ git commit -m "fix bug 101"
[issue-101 cc17032] fix bug 101
1 file changed, 1 insertion (+), 1 deletion(-1)
```
修复完毕后，切换到**master** 分支，并完成合并，最后删除**issue-101**分支：
```
 $git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 2 comits.
 $git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
readme.txt | 2+-
1 file changed, 1 insertion(+), 1 deletion(-)
 $ git branch -d issue-101
Deleted branch issue-101 (was cc17032).
```
这样原计划的bug修复完成，接下来回到**dev**分支干活！
```
 $git checkout dev
Switched to branch 'dev'
 $ git status
 #on branch dev
nothing to commit (working directory clean)
```
工作区是干净的，刚才的工作现场需要用**git stash list** 命令看看：
```
 $ git stash list
stash @{0}: WIP on dev: 6224937 add merge
```
工作现场还在，Git把stash内容存在某个地方了，需要恢复一下，有两个办法：
1、用**git stash apply**恢复，但恢复后，stash内容并不删除，需要用**git stash drop**来删除；
2、用**git stash pop**, 恢复的同时把stash内容也删了。
```
 $ git stash pop
#on branch dev
#....
#
Dropped refs/stash@{0} (f624...dd40)
```
再用**git stash list**查看，就看不到任何stash内容了：
```
 $git stash list
```
可以多次stash，恢复的时候需要先用**git stash list**查看，然后恢复指定的stash，用命令：
```
 $ git stash apply stash@{0}
```
### Feature 分支
 
在软件开发中，新的功能会不断的添加，而新功能的添加过程中，我们不希望因为一些实验性质的代码，而把主分支搞乱了，所以每添加一个新的功能，最好新建一个feature分支，在上面开发，完成后合并，最后删除该feature分支。以下是添加新分支以及合并或者删除的示例：

开发一个代号为Vulcan的新功能：
```
 $git checkout -b feature-vulcan
Switch to a new branch 'feature-vulcan'
```
开发完毕后，显示：
```
 $git add vulcan.py
 $git status
#on branch feature-vulcan
#...
#..
$ git commit -m "add feature vulcan"
[feature-vulcan 756d4af] add feature vulcan
1 file changed, 2 insertions(+)
create mode 100644 vulcan.py
```
切回**dev**， 准备合并：
```
 $git checkout dev
```
一切顺利的话，feature分支和bug分支是类似的，合并，然后删除；但若需要取消新功能，即删除掉该分支，则按照以下命令：
```
 $git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'
```
销毁失败，Git友情提示：**feature-vulcan**分支还没有被合并，若删除，将丢失修改，若需要强行阐述，则需要命令**git branch -D feature-vulcan**，现在强行删除：
```
 $ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 756d4af).
```
删除成功！

### 多人合作

当从远程仓库克隆时，实际上Git自动把本地的**master**分支和远程的**master**分支对应起来了，并且远程仓库的默认名称为**origin**，要查看远程库的信息，用**git remote**：
```
 $ git remote
origin
```
或者，使用**git remote -v**显示更加详细的信息：
```
 $ git remote -v 
origin git@github.com:michaelliao/learngit.git (fetch)
origin git@github.com:michaelliao/learngit.git (push)
```
上面显示了可以抓取和推送的**origin**的地址，若没有推送权限，就看不到push的地址。

#### 推送分支
推送分支就是把该分支上所有本地提交推送到远程库，推送时需要指定本地分支，这样Git就会把该分支推送到远程库对应的远程分支上，如：
```
 $ git push origin master
```
若要推送其他分支，比如**dev**，就改成：
```
 $ git push origin dev
```
但是，并不是一定要把本地分支往远程推送，那么哪些分支需要推送，哪些不需要呢？
1. **masterr** 分支是主分支，因此需要时刻与远程同步；
2. **dev**分支是开发分支，团队成员需要在上面工作，因此也需要与远程同步；
3. bug分支只用于在本地修复bug，就没有必要推到远程了，除非需要查看每周修复的bug数量；
4. feature分支是否推到远程，取决于是否和合作者在上面开发。
总之，在Git中，分支完全可以在本地自己藏着玩，是否推送，视自己的心情而定。

#### 抓取分支
多人协作时，大家都会往**master**和**dev**分支上推送各自的修改，现在模拟一个同伴，可以在另一台电脑上（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：
```
 $ git clone git@github.com:michaelliao/lerngit.git
Cloning into 'learngit'...
remote: Counting objects: 46, done.
remote: Compressing objects:100％ (26/26), done
remote: Total 46 (delta 16), reuse 45 (delta 15)
Receiving objects: 100% (46/46), 15.69 KiB | 6 KiB/s, done
Resolving deltas:100％　(16／16），done.
```
当同事从远程库clone时，默认情况下，他只能看到本地的master分支，可以如下查看：
```
 $git branch 
* master
```
现在，若他要在**dev**分支上开发，就必须创建远程**origin** 和**dev**分支到本地，于是他需要用这个命令创建本地**dev**分支：
```
 $git checkout -b dev origin/dev
```
现在，他就可以在**dev**上继续修改，然后时不时的把**dev**分支**push**到远程。
```
 $ git commit -m "add/usr/bin/env"
[dev 291bea8] add/usr/bin/env
1 file changed, 1 insertion(+)
 $ git push origin dev
Counting objects：5, done.
Delta compression using up to 4 threads.
Compressing objects:　100％ (2/2), done.
Writing objects: 100% (3/3), 349 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
fc38031..291bea8 dev-> dev
```
若同事已经向**origin/dev**分支推送了他的提交，而恰巧你也对同样的文件做了修改，试图推送：
```
 $git add hello.py
 $git commit -m "add coding: utf-8"
[dev bd6ae48] add coding: utf-8
1 fiel changed, 1 insertion (+)
 $git push origin dev
To git@githum.com:michealliao/learngit.git
! [rejected] dev -> dev (non-fast-forward)
erro: failed to push some refs to 'git@githum.com:michealliao/learngit.git'
hint: Updates were rejected because the tip of your corrent branch is behind
hint: its remote conterpart. Merge the remote changes (e. g. 'git pull' )
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push - help ' for details.
```
推送失败，因为同事最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，可以先用**git pull**把最新的提交从**origin/dev**抓下来，然后在本地合并，解决冲突后再推送：
```
 $ git pull
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
fc38031..291bea8 dev -> origin/dev
There is no tracking information for the current branch.
Please specifiy with branch you want to merge with.
See git-pull(1) for details
git pull <remote> <branch>
If you wish to set tracking ifnormation for this branch you can do so with:
git branch -- set-upstream dev origin/<branch>
```
**git pull** 也失败了，原因是没有指定本地**dev**分支与远程**origin/dev**分支的链接，依据提示，设置**dev**与**origin/dev**的链接：
```
 $git branch --set-upstream dev origin/dev
Branch dev set up to track remote branch dev from origin.
```
再pull：
```
 $git pull
Auto -merging hello.py
CONFLICT(content): Merge conflict in hello.py
Automatic merge failed: fix conflicts and then commit the results.
```
这回**git pull**成功，但合并有冲突，需要手动解决，解决的办法和分支管理的解决冲突完全一样，解决后，提交，再push。
```
 $ git commit -m ""merge & fix hello.py"
[dev adca45d] merge & fix hello.py
 $git push origin dev
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects:　100% (5/5), doen.
Writing objects: 100% (6/6), 747 butes, done.
Total 6 (delta 0), reused 0 (delta 0)
To git@githum.com:michealliao/learngit.git
291bea8..adca45d dev -> dev
```
因此，多人协作的工作模式通常是这样的：1、首先，可以试图用**git push origin branch-name** 推送自己的修改；2、若推送失败，则因为远程分支比你的本地更新，需要先用**git pull**试图合并；3、若合并有冲突，则解决从图，并在本地提交；4、没有冲突或者解决冲突后，在用**git push origin branch-name**推送就能成功！若**git pull**提示“no tracking information”，则说明本地分支和远程分支的连接关系没有创建，用命令**git branch --set-upstream branch-name origin/branch-name**。

# 标签管理
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样唯一确定了打标签时刻的版本。奖励无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来，所以标签也是版本库的一个快照。
Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（分支可以移动，但标签不能移动），所以创建和和删除标签也是瞬间完成的。tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

### 创建标签
在Git中打标签非常简单，首先，切换到需要打标签的分支上：
```
 $git branch
* dev
master
 $git checkout master
Switched to branch 'master'
```
然后，敲命令**git tag <name>**就可以打一个新标签，然后用命令**git tag**查看所有的标签。
```
 $ git tag v1.0
 $ git tag
v1.0
```
默认标签是打在最新提交的commit上的，有时候忘记打标签，如何处理？方法是找到历史提交的commit id，然后打上就可以了：
```
 $ git log --pretty =online --abbrev -commit
6a5819e merged bug fix 101
cc17032 fix bug 101
7825a50 merge with no-ff
6224937 add merge
59bc1cb conflict fixed
400b400 & simple
75a857c AND simple
fec145a branch test
d17efd8 remove test.txt
...
```
比方说要对**add merge** 这次提交打上标签，他对应的commit id 为6224937，敲入命令:
```
 $ git tag v0.9 6224937
```
再用命令**git tag** 查看标签：
```
 $ git tag 
v0.9
v1.0
```
注意，标签不是按照时间顺序列出，而是按照字母排列的，可以用**git show <tagname>**查看标签的详细信息：
```
$ git show v0.9
commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 11:22:08 2013 +0800
    add merge
...
```
可以看到，**v0.9**确实打在**add merge**这次提交上。还可以创建带有说明的标签，用**-a**指定标签名，**-m**指定说明文字：
```
$ git tag -a v0.1 -m "version 0.1 released" 3628164
```
用命令**git show <tagname>**就可以看到说明文字。
```
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 26 07:28:11 2013 +0800
version 0.1 released
commit 3628164fb26d48395383f8f31179f24e0882e1e0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 15:11:49 2013 +0800
    append GPL
```
还可以通过**-s**用私钥签名一个标签：
```
$ git tag -s v0.2 -m "signed version 0.2 released" fec145a
```
签名采用PGP签名，因此必须首先安装gpg（GnuPG），若没有找到gpg，或者没有gpg密钥对，就会报错：
```
gpg: signing failed: secret key not available
error: gpg failed to sign the data
error: unable to sign the tag
```
如果报错，请参考GnuPG帮助文档配置Key。用命令**git show <tagname>**可以看到PGP签名信息：
```
$ git show v0.2
tag v0.2
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Mon Aug 26 07:28:33 2013 +0800

signed version 0.2 released
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.12 (Darwin)

iQEcBAABAgAGBQJSGpMhAAoJEPUxHyDAhBpT4QQIAKeHfR3bo...
-----END PGP SIGNATURE-----
commit fec145accd63cdc9ed95a2f557ea0658a2a6537f
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 10:37:30 2013 +0800
    branch test
```
用PGP前面的标签是不可伪造的，因为可以验证PGP前面，验证签名的方法比较复杂，在此不做介绍。

### 操作标签

若标签打错了，也可以删除：
```
 $ git tag -d v0.1
Deleted tag 'v0.1' (was e078af9)
```
因为创建的标签都值存储在本地，不会自动推送到远程，所以，打错的标签可以在本地安全的删除。若要推送某个标签到远程，使用命令：**git push origin <tagname>**，或者，一次性推送全部尚未推送的到远程的标签：
```
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0

```
```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 554 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:michaelliao/learngit.git
 * [new tag]         v0.2 -> v0.2
 * [new tag]         v0.9 -> v0.9
```
若标签已经推送到远程，则要删除远程的标签就麻烦一些，先从本地删除，然后再从远程删除，删除命令也是push，但格式如下：
```
$ git tag -d v0.9
Deleted tag 'v0.9' (was 6224937)
```
```
$ git push origin :refs/tags/v0.9
To git@github.com:michaelliao/learngit.git
 - [deleted]         v0.9

```
要查看是否真的从远程库中删除了标签，可以登录到GitHub查看。

# 使用GitHub

我们一直用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

在GitHub出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难，因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。

但是在GitHub上，利用Git极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。

如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页[https://github.com/twbs/bootstrap](https://github.com/twbs/bootstrap)，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

```
git clone git@github.com:michaelliao/bootstrap.git

```

一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

Bootstrap的官方仓库`twbs/bootstrap`、你在GitHub上克隆的仓库`my/bootstrap`，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

![github-repos](https://www.liaoxuefeng.com/files/attachments/001384926554932eb5e65df912341c1a48045bc274ba4bf000/0)

如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

如果你没能力修改bootstrap，但又想要试一把pull request，那就Fork一下我的仓库：[https://github.com/michaelliao/learngit](https://github.com/michaelliao/learngit)，创建一个`your-github-id.txt`的文本文件，写点自己学习Git的心得，然后推送一个pull request给我，我会视心情而定是否接受。

# 使用码云
使用GitHub时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况（原因你懂的）。

如果我们希望体验Git飞一般的速度，可以使用国内的Git托管服务——[码云](https://gitee.com)（[gitee.com](https://gitee.com)）。

和GitHub相比，码云也提供免费的Git仓库，并且，免费版本还包含私有库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5人以下小团队免费。

使用码云和使用GitHub类似，我们在码云上注册账号并登录后，需要先上传自己的SSH公钥。选择右上角用户头像 -> 菜单“修改资料”，然后选择“SSH公钥”，填写一个便于识别的标题，然后把用户主目录下的`.ssh/id_rsa.pub`文件的内容粘贴进去：

![gitee-add-ssh-key](https://www.liaoxuefeng.com/files/attachments/0015014623796132cd9d2a2bdef4efd800ffa0e1df42964000/l)

点击“确定”即可完成并看到刚才添加的Key：

![gitee-key](https://www.liaoxuefeng.com/files/attachments/0015014624998255334476dc4994c0ab6e6057be4c5c7fe000/l)

如果我们已经有了一个本地的git仓库（例如，一个名为learngit的本地库），如何把它关联到码云的远程库上呢？

首先，我们在码云上创建一个新的项目，选择右上角用户头像 -> 菜单“控制面板”，然后点击“创建项目”：

![gitee-new-repo](https://www.liaoxuefeng.com/files/attachments/00150146266854163b62c2574ae45569179a3d22b479a4b000/l)

项目名称最好与本地库保持一致：

然后，我们在本地库上使用命令`git remote add`把它和码云的远程库关联：

```
git remote add origin git@gitee.com:liaoxuefeng/learngit.git

```

之后，就可以正常地用`git push`和`git pull`推送了！

如果在使用命令`git remote add`时报错：

```
git remote add origin git@gitee.com:liaoxuefeng/learngit.git
fatal: remote origin already exists.

```

这说明本地库已经关联了一个名叫`origin`的远程库，此时，可以先用`git remote -v`查看远程库信息：

```
git remote -v
origin    git@github.com:michaelliao/learngit.git (fetch)
origin    git@github.com:michaelliao/learngit.git (push)

```

可以看到，本地库已经关联了`origin`的远程库，并且，该远程库指向GitHub。

我们可以删除已有的GitHub远程库：

```
git remote rm origin

```

再关联码云的远程库（注意路径中需要填写正确的用户名）：

```
git remote add origin git@gitee.com:liaoxuefeng/learngit.git

```

此时，我们再查看远程库信息：

```
git remote -v
origin    git@gitee.com:liaoxuefeng/learngit.git (fetch)
origin    git@gitee.com:liaoxuefeng/learngit.git (push)

```

现在可以看到，origin已经被关联到码云的远程库了。通过`git push`命令就可以把本地库推送到Gitee上。

有的小伙伴又要问了，一个本地库能不能既关联GitHub，又关联码云呢？

答案是肯定的，因为git本身是分布式版本控制系统，可以同步到另外一个远程库，当然也可以同步到另外两个远程库。

使用多个远程库时，我们要注意，git给远程库起的默认名称是`origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

仍然以`learngit`本地库为例，我们先删除已关联的名为`origin`的远程库：

```
git remote rm origin

```

然后，先关联GitHub的远程库：

```
git remote add github git@github.com:michaelliao/learngit.git

```

注意，远程库的名称叫`github`，不叫`origin`了。

接着，再关联码云的远程库：

```
git remote add gitee git@gitee.com:liaoxuefeng/learngit.git

```

同样注意，远程库的名称叫`gitee`，不叫`origin`。

现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

```
git remote -v
gitee    git@gitee.com:liaoxuefeng/learngit.git (fetch)
gitee    git@gitee.com:liaoxuefeng/learngit.git (push)
github    git@github.com:michaelliao/learngit.git (fetch)
github    git@github.com:michaelliao/learngit.git (push)

```

如果要推送到GitHub，使用命令：

```
git push github master

```

如果要推送到码云，使用命令：

```
git push gitee master

```

这样一来，我们的本地库就可以同时与多个远程库互相同步：

![multi-remote](https://www.liaoxuefeng.com/files/attachments/001501462090750dbdbfd0431624ea09b2f5dd88b7b8e57000/m)

码云也同样提供了Pull request功能，可以让其他小伙伴参与到开源项目中来。你可以通过Fork我的仓库：[https://gitee.com/liaoxuefeng/learngit](https://gitee.com/liaoxuefeng/learngit)，创建一个`your-gitee-id.txt`的文本文件， 写点自己学习Git的心得，然后推送一个pull request给我，这个仓库会在码云和GitHub做双向同步。

# 自定义Git

在[安装Git](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)一节中，我们已经配置了`user.name`和`user.email`，实际上，Git还有很多可配置项。

比如，让Git显示颜色，会让命令输出看起来更醒目：

```
$ git config --global color.ui true

```

这样，Git会适当地显示不同的颜色，比如`git status`命令：

![git-color](https://www.liaoxuefeng.com/files/attachments/0013849265828833012fe6261a54c5794959d6c1883590b000/0)

文件名就会标上颜色。

### 忽略特殊文件
有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次`git status`都会显示`Untracked files ...`，有强迫症的童鞋心里肯定不爽。

好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)

忽略文件的原则是：

1.  忽略操作系统自动生成的文件，比如缩略图等；
2.  忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3.  忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有`Desktop.ini`文件，因此你需要忽略Windows自动生成的垃圾文件：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

```

然后，继续忽略Python编译产生的`.pyc`、`.pyo`、`dist`等文件或目录：

```
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

```

加上你自己定义的文件，最终得到一个完整的`.gitignore`文件，内容如下：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa

```

最后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了：

```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.

```

如果你确实想添加该文件，可以用`-f`强制添加到Git：

```
$ git add -f App.class

```

或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

```
$ git check-ignore -v App.class
.gitignore:3:*.class    App.class

```

Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

### 配置别名
有没有经常敲错命令？比如`git status`？`status`这个单词真心不好记。

如果敲`git st`就表示`git status`那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后`st`就表示`status`：

```
$ git config --global alias.st status

```

好了，现在敲`git st`看看效果。

当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

```
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch

```

以后提交就可以简写成：

```
$ git ci -m "bala bala bala..."

```

`--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

在[撤销修改](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374831943254ee90db11b13d4ba9a73b9047f4fb968d000)一节中，我们知道，命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个`unstage`别名：

```
$ git config --global alias.unstage 'reset HEAD'

```

当你敲入命令：

```
$ git unstage test.py

```

实际上Git执行的是：

```
$ git reset HEAD test.py

```

配置一个`git last`，让其显示最后一次提交信息：

```
$ git config --global alias.last 'log -1'

```

这样，用`git last`就能显示最近一次的提交：

```
$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py

```

甚至还有人丧心病狂地把`lg`配置成了：

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

```

来看看`git lg`的效果：

![git-lg](https://www.liaoxuefeng.com/files/attachments/00138492662982594cbd1a942114472aeeb5f0a502faed1000/0)

为什么不早点告诉我？别激动，咱不是为了多记几个英文单词嘛！

### 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

```
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1

```

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

```
$ cat .gitconfig
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com

```

配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

### 搭建Git服务器
在[远程仓库](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000)一节中，我们讲了远程仓库实际上和本地仓库没啥不同，纯粹为了7x24小时开机并交换大家的修改。

GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

假设你已经有`sudo`权限的用户账号，下面，正式开始安装。

第一步，安装`git`：

```
$ sudo apt-get install git

```

第二步，创建一个`git`用户，用来运行`git`服务：

```
$ sudo adduser git

```

第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

```
$ sudo git init --bare sample.git

```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

```
$ sudo chown -R git:git sample.git

```

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash

```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：

```
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.

```

剩下的推送就简单了。

### 管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的`/home/git/.ssh/authorized_keys`文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

这里我们不介绍怎么玩[Gitosis](https://github.com/res0nat0r/gitosis)了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

### 管理权限

有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。[Gitolite](https://github.com/sitaramc/gitolite)就是这个工具。

这里我们也不介绍[Gitolite](https://github.com/sitaramc/gitolite)了，不要把有限的生命浪费到权限斗争中。

Git虽然极其强大，命令繁多，但常用的就那么十来个，掌握好这十几个常用命令，你已经可以得心应手地使用Git了。

友情附赠国外网友制作的Git Cheat Sheet，建议打印出来备用：

[Git Cheat Sheet](https://pan.baidu.com/s/1kU5OCOB#path=%252Fpub%252Fgit)

现在告诉你Git的官方网站：[http://git-scm.com](http://git-scm.com)，英文自我感觉不错的童鞋，可以经常去官网看看。什么，打不开网站？相信我，我给出的绝对是官网地址，而且，Git官网决没有那么容易宕机，可能是你的人品问题，赶紧面壁思过，好好想想原因。
