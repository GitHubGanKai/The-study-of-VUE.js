# 最全的Git入门操作使用详解 #

	 git clone     git remote		 git fetchgit	 	pullgit push 

首先看一下,这个图片,介绍一下,我们编辑的地方就是工作区,workspace,当开发完了之后,

**第一步:**使用,`git add <文件名>`  添加文件到暂存区(stage),也就是下图中的index中.用 git add . 代表添加所有的文件;

**第二步:**使用 `git commit -m"填写你改动的内容"`,将代码提交到本地仓库中;

**第三步**使用`git pull origin master:master ` 将远程分支代码和本地分支代码合并;

**第四步:** 使用`git push origin master:master `将代码合并好的代码提交到远程代码仓库.

----------


![](./imgs/git-descript.jpg)

----------

# 一、git clone #
## 语法: $ git clone <版本库的网址> <本地目录名>



1. 一般来说,刚新加入一个项目组,在已经创建好的工程上进行开发,都是直接克隆代码下来的.一般是从远程主机上克隆一个版本库,一般来说,都是直接克隆下来的.`git clone <远程仓库地址URL>`,第二个参数如果不写,则默认和远程主机上的目录同名.
2. 克隆下来的代码里面有团队其他伙伴的在mast上建立的远程分支,如果你提前在网页上新建了分支,你可以用`git branch -r`查看远程所有的分支,你需要找到自己的分支,使用命令`git checkout <你要切换的分支名>`,切换到自己的分支上进行开发,**特别注意,克隆下来的代码一定要记得创建自己的分支,在自己的分支上进行开发,如果没有切换到自己的分支上,默认是在master上进行开发,到时候推送代码的时候,你会出现没有推送权限的**

*如果在克隆之前,你已经在网页上新建了一个远程的分支,比如叫做demo,克隆的时候可以直接用 git clone -b demo url 来将自己的远程仓库直接克隆下来*

----------

# 二、git remote #
## 语法:remote 是远程的意思

为了便于管理,git要求每个g远程主机必须指定一个主机名,git remote就是用于管理主机名的,`git remote`没有参数的时候,就是默认列出所有的主机名,

	$ git remote
	
	origin
如果是带参数 `git remote -v` 则可以查看远程主机的网址详细信息,如下:

	$ git remote -v
	origin  git@github.com:jquery/jquery.git (fetch)
	origin  git@github.com:jquery/jquery.git (push)
上面命令表示，当前只有一台远程主机，叫做origin，后面的是它的网址。

*git remote show命令加上主机名，可以查看该主机的详细信息*

	$ git remote show <主机名>

`git remote add` 命令用于添加远程主机。

	$ git remote add <主机名> <网址>

`git remote rm` 命令用于删除远程主机。

	$ git remote rename <原主机名> <新主机名>


----------
# 三、git fetch #

一旦远程主机的版本库有了commit更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令

##语法: $ git fetch <远程主机名> ##

上面命令将某个远程主机的更新，全部取回本地。git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。默认情况下，git fetch取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。

**比如，取回origin主机的master分支。**

	$ git fetch origin master
所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。

**git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支(远程本地都包括)。**

	$ git branch -r	

	origin/master       // origin是远程主机名 master是远程分支名

	$ git branch -a

git branch -a


	* master		// 带有*代表本地目前所在的分支  红色字体代表远程仓库分支,绿色字体代表本地仓库分支
	
	remotes/origin/master


上面命令表示，本地主机的当前分支是master，远程分支是origin/master。

取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。

	$ git checkout -b newBrach origin/master

上面命令表示，在origin/master的基础上，创建一个新分支。

此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。

	$ git merge origin/master
	# 或者
	$ git rebase origin/master
上面命令表示在当前分支上，合并origin/master

----------

#  四、git pull 将远程分支拉下来和本地分支合并#
git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。

	$ git pull <远程主机名> <远程分支名>:<本地分支名>

比如，取回origin主机的next分支，与本地的master分支合并，需要写成下面这样。

	$ git pull origin next:master

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

	$ git pull origin next

上面命令表示，取回origin/next分支，再与当前分支合并。**实质上，这等同于先做git fetch，再做git merge。**

	
	$ git fetch origin
	$ git merge origin/next

在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。

## Git也允许手动建立追踪关系。 ##
	git branch --set-upstream master origin/next
上面命令指定本地master分支追踪远程origin/next分支。

如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名。

	$ git pull origin

上面命令表示，本地的当前分支自动与对应的origin主机"追踪分支"（remote-tracking branch）进行合并。

如果当前分支只有一个追踪分支，连远程主机名都可以省略。

	$ git pull
上面命令表示，当前分支自动与唯一一个追踪分支进行合并。

如果合并需要采用rebase模式，可以使用--rebase选项。

	$ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
如果远程主机删除了某个分支，默认情况下，git pull 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。

但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的分支。

	$ git pull -p
	# 等同于下面的命令
	$ git fetch --prune origin 
	$ git fetch -p

----------
# 五、git push #

git push命令用于将本地分支的更新，推送到远程主机。它的格式与git pull命令相仿。

	$ git push <远程主机名> <本地分支名>:<远程分支名>
注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。

如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。

	$ git push origin master

**上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。**


	$ git push origin :master
	# 等同于
	$ git push origin --delete master

## 上面命令表示删除origin主机的master分支。 ##

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

	$ git push origin
上面命令表示，将当前分支推送到origin主机的对应分支,如果当前分支只有一个追踪分支，那么主机名都可以省略。

	$ git push
如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。

	$ git push -u origin master
上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。

不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。
	$ git config --global push.default matching
	# 或者
	$ git config --global push.default simple

还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用--all选项。
	
	$ git push --all origin

上面命令表示，将所有本地分支都推送到origin主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用--force选项。

	$ git push --force origin 

上面命令使用--force选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用--force选项。

最后，git push不会推送标签（tag），除非使用--tags选项。

	$ git push origin --tags


# git 在使用过程中遇见的一些错误 # #

1. Git push时出现"The remote end hung up unexpectedly"错误
在使用git管理本地项目时，配置好了所有环境并完成add和commit的操作后却迟迟无法push成功，错误显示为：

	 The remote end hung up unexpectedly

经过一番查找资料后发现，是需要push 的内容太多（超过了100M）。在使用https进行上传的时候由于糟糕的网速而导致上传的时间过长，从而使得push失败。

建议在使用git进行push/pull操作时还是使用ssh方式，速度相对较快。同时可以通过命令"git config ssh.postBuffer 524288000"的方式提高buffer的容量。

# git 如何push 大文件? #
Git LFS的官方网址在这里： [https://git-lfs.github.com/](https://git-lfs.github.com/)，官网上有很详细的说明，现在来简单说下使用方式：先安装 Git LFS 的客户端，**然后在将要push的仓库里重新打开一个bash命令行：**

- 只需设置1次 LFS : git lfs install
- 然后 跟踪一下你要push的大文件的文件或指定文件类型 git lfs track "*.pdf" ， 当然还可以直接编辑.gitattributes文件
- 以上已经设置完毕， 其余的工作就是按照正常的 add , commit , push 流程就可以了 : 
git add yourLargeFile.pdf
git commit -m "Add Large file"
git push -u origin master

**目前 Git LFS的总存储量为1G左右，超过需要付费。**