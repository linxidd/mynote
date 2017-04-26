#Pro Git 笔记
##一、Git第一步配置
1、通过```git config```命令配置全局环境，相关配置文件在三个位置`/etc/gitconfig`全局配置文件可以使用`--system`选项配置、`~/.gitconfig`或者`~/.config/git/config`当前用户的配置文件存放地点使用`--global`选项配置、还可以存放在当前git文件夹的.git/config文件中特指该仓库的配置文件。  
2、每一级的配置都可以覆盖上一级的配置值。  
3、身份验证  配置用户名和邮箱地址。  
    `$ git config --global user.name "name"`  
    `$ git config --global user.email " "`  
4、配置git默认编辑器  
`$ git config --global core.editor vim`  
5、检查配置设置   
`$ git config --list`  
也可以检查某配置项  
`$ git config user.name`  
6、帮助文件  
`$ git help <verb>`  
`$ git <verb> --help`  
`$ man git-<verb>`  
##二、Git基础
###获取Git仓库  
####1、初始化仓库目录  
`$ git init`  
创建一个新的目录`.git`，包含Git仓库结构包含所有必须的文件，如果想立即追踪仓库中的文件可以执行一下命令  
`$ git add *.c`  
`$ git add LICENSE`  
`$ git commit -m 'initial project version`  
###2、克隆存在的仓库  
`git clone`命令获取仓库中的所有数据。文件的所有历史版本都被拉下来。
重新命名仓库名称  
`$ git clone <url> <name>`  
除了HTTP之外还有`git://`和`user@server:path/to/repo.git`的SSH方式。
####3、在仓库中记录变化  
目录中的文件分为追踪和未追踪的两类。  
1、检查文件状态  
`$ git status`
2、追踪新文件  
`$ git add <filename>`  
3、简短的状态  
`$ git status -s`  
4、忽略文件  
编写`.gitignore`文件  
`$ cat .gitignore`  
一般需要忽略log,tmp,pid目录  
`.gitignore`文件匹配规则  
- 空行和#开头的行  
- 标准blog匹配  
- 在匹配项后加/表示一个目录  
- 使用！反向匹配  
####浏览staged和unstaged变化
`$ git diff`比`$ git status`更详细的变更信息  
`--staged`可以列出staged变换和最后commit的差异  

####commit变化
`$ git commit`打开commit文件输入commit message，也可以使用`-v`命令在commit文件中列出diff  
`$ git commit -m <message>`  
####跳过staged步骤
` $ git commit -a -m <message>`  
####移除文件  
`$ git rm <filename>`  
下一次commit的时候删除，如果已经修改并且add需要使用`-f`选项。  
如果需要在本地保留在stage中删除可以使用`--cached`选项。
`git rm log/\*.log`  
`git rm \*~`
####移动文件  
重命名文件使用  
`$ git mv file_from file_to`  
###4、查看commit历史  
`$ git log`  
`-p`参数列出diff记录，还可以使用`-num`命令列出前num个  
`--stat`命令列出修改的文件和修改的行数  
`--pretty=<option>`可以使用`oneline`,`short`,`full`,`fuller`列出commit记录  
还可以使用`format`格式，ex  
`$ git log --pretty=format:"%h - %an,%ar : %s"  
`--graph`使用ASCII图显示  
####限制log输出  
`$ git log --since=2.weeks`  --author --grep --all-match  
`$ git log -Sfuction_name`  
EX:
`$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges -- t`  
###5、撤销  
`$ git commit -amend`  
常用做法  
`$ git commit -m 'initial commit'`  
`$ git add forgotten_file`  
`$ git commit --amentd`  
####unstaging
`$ git reset HEAD <file_name>`  
####unmodifying  
`$ git checkout -- <file_name>`  
###6、使用远端仓库
####显示远端
`$ git remote` `$ git remote -v`显示地址  
####添加远程仓库  
`$ git remote add [shortname] [url]`  
`$ git fetch [shortname]`  拉取shortname的数据  
####拉取你的远端  
`$ git fetch [remote-name]`  
拉取远端所有数据，并带有远端所有引用分支，你可以在任何时候合并或者查看。  
如果clone了一个远端仓库，这个远端仓库默认为"origin"。所以`get fetch origin`可以拉取所有clone以来的所有提交的数据。*需要注意的是所有`git fetch`操作都不会合并你的本地仓库或者改变你的本地数据。你必须手动合并。*  
如果有一个分支用于追踪远端分支，你可以使用`git pull`命令自动的拉取然后合并远端分支到你当前的分支。`git clone`默认自动设置你的本地master分支追踪远端的master分支。  
####提交到远端  
`git push [remote-name] [branch-name]`  
如果需要提交master分支到origin远端使用  
`git push origin master`  
这个命令只有你对服务器内容有访问权限并且中间没有其他人提交数据。  
####检查远端  
`$ git remote show origin`  
####移除和重命名远端  
`$ git remote renanme `  
`$ git remote rm`  
###7、标签
####列出标签
`$ git tag`
`$ git tag -l 'v1.8.5*'`  
####创建标签  
git有两类标签：轻量和注释  
####注释标签  
`$ git tag -a v1.4 -m 'my version 1.4'`  
####轻量标签  
`$ git tag v1.4-lw`  
####分享标签 
`$ git push origin v1.5`  
`$ git push origin --tags`  
###8、Git重命名  
`$ git config --global alias.co checkout`  
`$ git config --global alias.br branch`  
`$ git config --global alias.ci commit`  
`$ git config --global alias.st status`  
`$ git config --global alias.unstage 'reset HEAD --'`
##三、Git分支  
*充分认识HEAD在分支管理中的作用，切换分支也就是切换HEAD*  
####创建新分支  
`$ git branch <branch_name>`  
####切换分支  
`$ git checkout <branch_name>`  
##注意：切换分支会改变你的工作目录  
`$ git log --oneline --decorate --graph --all`  
##分支基础
`$ git checkout -b iss53`   
这个命令是  
`$ git branch iss53`  
`$ git checkout iss53`  
切换到这个issue分支  
`$ git checkout master`  
`$ git merge hotfix`  
将issue合并到master 分支  






