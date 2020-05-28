## GitHub进阶笔记

---

### 图形界面工具

> Git GUI
>
> Source Tree
>
> EGit
>
> > 这三个图形界面，自己拓展



### .gitignore

#### 使用场合

> 1.忽略操作系统自动生成的文件
>
> > 比如：缩略图等
>
> 2.忽略编译生成的中间文件、可执行文件等
>
> > 比如：c语言编译产生的.obj文件和.exe文件；
>
> 3.忽略自己带有敏感信息的配置文件
>
> > 比如：存放口令的配置文件
>
> 4.tmp/	临时文件
>
> 5.log/	日志文件
>
> 等等

#### 如何使用

##### 创建.gitignore文件

> 在Git Bash命令模式中，使用**vim**创建一个名为**`.gitignore`**的文件
>
> **每一行**即表明要忽略版本控制的一个/类文件
>
> > 支持**通配符**的使用

##### 查看是哪条规则忽略了某一文件

> `git check-ignore -v project.txt`

##### 将.gitignore忽略的文件加入控制

> `git add -f 文件名`
>
> > `git add -f project.txt`

> 注
>
> > `.gitignore`中每一行的文件会被自动忽略
> >
> > `.gitignore`文件本身默认不跟踪
> >
> > > `git status` `.gitignore`文件显示为红色

###### 案例

.gitignore

```
# 忽略ignoretest文件/目录
ignoretest
# 忽略project.txt文件
project.txt
```



##### github提供的模板

> 新建仓库 --> Add .gitignore:None --> 选择对应的模板

> GitHub官方托管的gitignore仓库
>
> > `https://github.com/github/gitignore`



### 换行处理

> CR：carriage return回车，光标移到首行
>
> > '\r' = return
>
> LF：line feed换行，光标下移一行
>
> > ‘\n’ = newline

##### linux换行

> `\n`

##### windows换行

> `\r\n`

##### MAC OS换行

> `\r`

##### 配置

> `git config --global core.autocrlf true`
>
> > 提交时转换为LF，检出时转为`CRLF`
> >
> > **默认设置不用修改**
>
> `git config --global core.safecrlf false`
>
> > 允许提交时包含混合换行符的文件
> >
> > > 添加之后关于**换行的警告消失**
> >
> > > 输入 `git config -l`
> > >
> > > > 发现配置中多了`core.safecrlf=false`



### 别名

#### 以图形的方式打印Git提交的日志

> > `git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short`
> >
> > > %h：hash值
> > >
> > > %ad：提交commat信息
> > >
> > > %s%d：提交日期
> > >
> > > [%an]：提交人
> > >
> > > --graph：以图形化的方式显示
> > >
> > > --data=short：以短日期的方式显示

#### 设置别名

##### git别名配置文件

> `cd`
>
> > 进入到git的主目录
>
> `ls -l -a`
>
> > 查看文件，`.gitconfig`文件存储
>
> 所有别名可以在`.gitconfig`文件中查看

###### .gitconfig

```
[user]
        name = wangxinhui
        email = 1076710856@qq.com
[core]
        safecrlf = false
[alias]
        mylog = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
```



### 凭证

##### https协议凭证

> 存储凭证
>
> > `git config --global credential.helper wincred`
> >
> > > wincred：会将第一次输入的用户名密码存起来，之后提交就不会再提示我们输入用户名和密码
> >
> > > 可在`~/.gitconfig`文件中查看配置



### Git协议

#### 本地协议（一般不使用）

##### 克隆本地仓库

> `git clone /c/wd/test.git`
>
> > 克隆本地仓库
>
> `git clone file:///c/wd/test.git`
>
> > 克隆本地仓库，不建议使用file://
> >
> > > 添加了file之后，所有的操作都是通过**协议栈**的方式来做协议的传输，相对而言比较慢
>
> 添加远程仓库的链接
>
> > `git remote add origin /c/wd/test.git`

###### 缺点

> 没有网络，无法在互联网中访问



#### Git协议（速度最快）

> `git clone git://setver_ip/test.git`
>
> > 克隆远程仓库
>
> `git remote add origin git://server_ip/test.git`
>
> > 添加远程仓库的链接 

###### 缺点

> 权限要么对所有人开放，要么对所有人不开放



#### HTTP协议（主流）

> `git clone https://github.com/minnersun/git_study`
>
> > 克隆远程仓库
>
> `git remote add origin https://github.com/minnersun/git_study`
>
> > 添加远程仓库链接

###### 缺点

> HTTP协议传输效率比较低
>
> > HTTP为超文本传输协议，需要对数据进行压缩，解压

> 从安全性角度来讲，和SSH协议差不多
>
> > 不适用存储凭证，需要输入用户名和密码



#### SSH协议(重点)

##### 生成密钥对

> `ssh-Keygen -t rsa -C "your email"`
>
> > rsa：加密算法
> >
> > your email：github注册使用的email地址
>
> 案例
>
> > `ssh-Keygen -t rsa -C "1076710856@qq.com"`
> >
> > > 将密钥对存储到默认位置即可，全部默认下一步
>
> 位置
>
> > `cd ~`
> >
> > `cd .ssh`
> >
> > > `id_rsa`
> > >
> > > > 私钥
> > >
> > > `id_rsa.pub`
> > >
> > > > 公钥
> >
> > > `cat id_rsa.pub`
> > >
> > > > 将公钥打印出来，复制

##### github配置公钥

> 点击右侧头像 --> Settings --> SSH and GPG keys --> New SSH key
>
> > Title
> >
> > > 给公钥起一个名字
> >
> > Key
> >
> > > 粘贴复制的公钥

> 配置完成之后即可用SSH协议对远程仓库进行操作



##### SSH方式克隆远程仓库

> `git clone ssh://@github.com/minnersun/git_study`
>
> `git clone git@github.com:minnersun/git_study`
>
> > 克隆远程仓库，一般写成简短的命令
> >
> > > 如果存在删除原有数据源
> > >
> > > > `git remote rm origin`

##### SSH方式添加远程仓库

> `git remote add origin git@github.com:wangding/test.git`
>
> > 添加远程仓库的链接





### Git基本操作

#### `git`

> 列出常用的git子命令的信息

#### `git help `

> `git help -a`：查看全部git子命令

#### `git blame`

###### 逐行查看文件的修改历史

> `git blame <file Name>`
>
> > 逐行查看文件的修改历史

###### 查看第10行开始，到100行

> `git blame -L 10,100 <file name>`
>
> > 从第10行开始，到100行。逐行查看文件的修改历史

#### `git clean `

###### 列出打算清除的档案

> `git clean -n`
>
> > 列出打算清除的档案
> >
> > > 即，还没有添加到暂存区的文件（**未进行add操作**）

###### 真正的删除这些文件

> `git clean -f`
>
> > 真正的删除这些文件

###### 连.gitignore中忽略的档案也删除

> `git clean -x -f`
>
> > 连.gitignore中忽略的档案也删除

#### `git status`

###### 简化`git status`列出的信息

> `git status -sb`简化`git status`列出的信息

#### `git rm `

> `git rm <file name>`
>
> > 删除已提交的文件
> >
> > > 案例
> > >
> > > > `git rm a.txt`

#### `git mv`

###### 改名

> `git mv a.txt b.txt`
>
> > 将`a.txt`改名为`b.txt`

###### 移动

> `git mv a.txt ./demo/`
>
> > 将`a.txt`文件移动到`./demo`文件夹下

#### `git add`

###### 将多个文件放入暂存区

> `git add a b c`
>
> > 将`a，b，c`三个文件放入暂存区中

###### 将当前目录存放到暂存区

> `git add .`
>
> > 将当前目录存放到暂存区中

###### 一个文件多次提交

> `git add -p`
>
> > 一个文件多次提交
>
> > 案例
> >
> > > `git add -p aaa.txt`
> > >
> > > > 提示信息：`Stage this hunk [y,n,q,a,d,s,e,?]?`
> > > >
> > > > > 是否作为一整块提交
> > > > >
> > > > > > y：yes，添加该段代码到暂存区
> > > > > >
> > > > > > n：no，不添加该段代码到暂存区
> > > > > >
> > > > > > q：quit，推出
> > > > > >
> > > > > > a：添加该文件所有的代码到暂存区
> > > > > >
> > > > > > d：不添加该文件所有的代码到暂存区
> > > > > >
> > > > > > s：切分（拆分当前代码块，并确认每一块是否添加暂存区）
> > > > > >
> > > > > > e：手工编辑块



### `git commit`

##### 跳过add，提交仓库

> `commit -a -m "message"` / `commit -am "message"`
>
> > 跳过暂存区(add)，前提：仓库至少提交过该文件一次

##### commit提交规范

> `<type>(scop):<subject> `		
>
> 空一行
>
> `<body>`			--可选
>
> 空一行
>
> `<footer>`		--可选

###### 头部

> `<type>`:哪一种提交类型		--必写
>
> > `feat`：新功能
> >
> > `fix`：修补bug
> >
> > `docs`：文档（documentation）
> >
> > `style`：格式（不影响代码运行的变动）
> >
> > `refactor`：重构（既不是新功能，也不是修改bug的代码边动）
> >
> > `test`：增加测试
> >
> > `chore`：构建过程或辅助工具的变动

> `<scop>`:影响范围	--可省
>
> > 比如数据层
> >
> > 控制层
> >
> > 视图层等等
> >
> > > 视项目不同而不同。

> `<subject>`:提交的主题	--可省
>
> > > 是 commit 目的的简短描述，不超过50个字符
> > >
> > > 第一个字母小写
> > >
> > > 结尾不加句号

###### body

> 对本次 commit 的详细描述，可分行、分段



### Git信息查看

##### `git status`

> `git status -sb`
>
> > 简短输出状态信息

##### `git show`

> `git show head`
>
> 查看当前版本信息
>
> > `head`：为指针，可以将 `head`想象为当前分支的别名
> >
> > > 相当于当前分支的hash值(自己的理解)
>
> `git show HEAD^` / `git show HEAD~1`
>
> > 前一个提交
> >
> > > `git show HEAD^^` / `git show HEAD~2`
> > >
> > > > 前两个提交

> `git show [版本号]`
>
> > 查看固定版本信

##### `git log`

> `git log <file name>`
>
> > 查看某一文件的日志信息

> `git log -- grep <msg>`
>
> > 对日志的某一信息进行过滤
>
> > 案例
> >
> > > `git log --grep "新增"`

> `git log -n`
>
> > 查看多少条日志

##### `git diff`

> `git diff`
>
> > 工作目录和暂存区的差异

> `git diff --cached`
>
> > 查看暂存区和最后提交版本的差异
>
> `git diff --cached HEAD~2`
>
> > 查看暂存区与前两个提交的差异

> `git diff efb9813 99754be`
>
> > 已提交版本间的差异

### 回撤操作

##### `git reset`

> `get reset HEAD`
>
> > 回撤暂存区(add)内容到工作目录

> `git reset HEAD --soft`
>
> > 回撤提交(commit)到暂存区(add)

> `git reset HEAD -hard`
>
> > 回撤提交(commit)，放弃变更

> `git push -f`
>
> > 回撤远程仓库，-f即--force
>
> > 执行此操作后，远程仓库也会回撤到本地版本
> >
> > > 应尽量避免回撤远程仓库

##### `git rebase`

> `git rebase -i HEAD~4`
>
> > 对前4个提交做变基操作
> >
> > > `pick 1464b9c rm
> > > pick 8e1709d aaa.txt
> > > pick ebea3e6 test
> > > pick 847e0b1 lll`
> > >
> > > > 修改
> > > >
> > > > > 如果删除上面的信息，则变基流产
> >
> > > 可选操作
> > >
> > > > p：使用commit
> > > >
> > > > r：使用commit，但编辑commit消息
> > > >
> > > > e：使用commit，但停止修改
> > > >
> > > > s：使用commit，但是混入前一个commit
> > > >
> > > > f：类似于s，但是放弃此提交的日志信息
> > > >
> > > > x：使用shell运行命令
> > > >
> > > > d：删除提交
> > > >
> > > > .......
> > >
> > > 案例
> > >
> > > > `pick 1464b9c rm
> > > > s 8e1709d aaa.txt
> > > > s ebea3e6 test
> > > > pick 847e0b1 lll`
> > > >
> > > > > 将2，3的提交，合并到1中
> > >
> > > > `r 1464b9c rm
> > > > r 8e1709d aaa.txt
> > > > r ebea3e6 test
> > > > r 847e0b1 lll`
> > > >
> > > > > 修改commit信息
> > > > >
> > > > > > wq保存后，在弹出的文本中重新编辑commit的内容
> >
> > 变基后的hash值会改变



### `git tag`

> 在某些历史版本上，有里程碑意义的
>
> > 在这些时间点上，可能意义重大，在这些节点上会打上标记
>
> > 在回溯的时候可以不用hash值，而使用标签值

##### 添加标签

###### `git tag`

> 查看，列出所有的标签

###### `git tag foo`

> 在HEAD的位置(当前位置)打上标签
>
> > 案例
> >
> > > `git tag v0.1`

###### `git tag foo -m "message"`

> 给当前提交打上标签，并给message信息注释

###### `git tag foo HEAD~4`

> 给当前提交之前的第四个版本上，打上标签

##### 推送标签到远程仓库

###### `git push origin --tags / git push origin v0.1`

> 把标签推送到远程仓库

##### 拽下远程仓库的标签

###### `git pull --tags`

> 将远程库的标签同步到本地

##### 删除本地标签

###### `git tag -d foo`

> 删除标签
>
> > `git tag -d v0.1`

##### 删除远程仓库标签

###### `git push origin : refs/tags/v0.1`



### git分支

> 分支之间并行开发，互不影响
>
> 可能会导致分支的滥用
>
> > 无法判断分支之间的关系，无法区分主干分支

##### 分支指令

###### 创建分支

> `git branch foo`
>
> > `(master)git branch iss53`
> >
> > > 为master创建名为iss53的分支

###### 进入分支

> `git checkout foo`
>
> > `git checkout iss53`
> >
> > > 进入iss53分支
>
> `git checkout -b foo`
>
> > 创建分支并进入

###### 合并分支

> `git merge foo`
>
> > `git merge hotfix`
> >
> > > 将hotfix分支合并到master分支中

###### 修改分支名字

> `git branch -m old_name newname`
>
> `git branch -M old_name newname

###### 删除分支

> `git branch -d hotfix`
>
> `git branch -D hotfix`
>
> > 删除本地分支
>
> `git push origin --delete iss53`
>
> > 删除远程分支

###### 查看分支列表

> `git branch`
>
> > 查看分支列表
>
> `git branch -r`
>
> > 列出远程分支
>
> `git branch -v`
>
> > 列出分支的名称，版本hash和message
>
> `git branch --merged`
>
> > 查看已合并的分支
>
> `git branch --no-merged`
>
> > 查看没有合并的分支
>
> `git branch -r -merged`
>
> > 列出远程合并的分支

###### 取出远程foo分支

> `git checkout -t origin/goo`

###### 推送分支到远程

> `git push -u origin <space>:<remote branch>`
>
> > `(foo)git push -u origin iss53`
> >
> > > 将iss53分支，推送到远程iss53上

###### 合并分支

> `git merge foo`



##### 分支示例详解

> `(master)git branch iss53`		--C2
>
> > 在master创建了一个新的分支（iss53）
> >
> > > iss53没有进行提交，iss53分支指针指向C2
> >
> > `(iss53)git commit`		--C3
> >
> > > 此时iss53分支指针指向C3
>
> `(master)git branch hotfix`	--C2
>
> > 在master创建了一个新的分支hotfix
> >
> > > hotfix没有提交，hotfix分支指针指向C2
> >
> > `(hotfix)git commit`	--C4
> >
> > > 此时hotfix分支指针指向C4
>
> `(master)git merge hotfix`		--master合并hotfix分支，master指针指向C4
>
> > 此时master的指针指向hotfix分支的提交(C4)
> >
> > > hotfix分支被master分支合并
>
> `(iss53)git commit`	--C5
>
> > 此时iss53分支的指针指向C5
>
> `(master)git merge iss53`	--master的C4和iss53分支的C5，合并为C6
>
> > master分支合并iss53分支
> >
> > 此时master指针指向C6
> >
> > > C6有两个parent：C4和C5

##### 冲突解决

###### 场景

> 1.在不同分支上修改同一文件
>
> 2.不同的人，修改了同一个文件
>
> 3.不同的仓库，修改了同一个文件
>
> > 冲突只在合并分支的时候才发生
> >
> > 冲突发生时，冲突的代码不会丢失
>
> > 解决冲突，重新提交，commit时不要给message

###### 冲突信息的格式

冲突信息

> 发生冲突的是master的HEAD，`=======`上面为`master`版本，下面为`iss53`版本

```
<<<<<<< HEAD
aaa--master
=======
aaac==iss53
>>>>>>> iss53
2222
2222
```

修改冲突

> 保留maste或者iss53其中一个版本，或者重新修改

```
1111--master and iss53
2222
2222
```



##### `git stash`

> 将工作进度放入暂存区
>
> > 如果工作完成未提交，不可以切换进入别的分支
> >
> > 可以将当前完成的工作放入暂存区中

###### 保存进度

> `git stash`
>
> > 将当前分支的工作进度保存进暂存区

###### 弹出进度

> `git stash pop`
>
> > 弹出暂存区中的工作进度

###### 查看stash列表

> `git stash list`

###### 删除stash列表

> `git stash clear`