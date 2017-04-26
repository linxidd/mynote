#Pro Git 笔记
----
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
####在仓库中记录变化  
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


