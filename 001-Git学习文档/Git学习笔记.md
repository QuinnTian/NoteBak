### **Git的三种状态及三个工作区**

#### **三个基本状态**
 - **已提交(committed)**
 表示数据已经安全的提交到本地仓库之中
 - **已修改(modified)**
 已经修改了文件，强调修改未保存
 - **已暂存(staged)**
 表示将被修改的文件做了标识
####  **三个工作区**
 - **Git仓库**
	 - 对应已提交(committed)状态
	 - 仓库中的文件的状态是**已提交状态**
 - **工作目录**
	 - 就是普通的目录
	 - 里面的文件可以自己修改，修改后为标识成**已修改状态**
 - **暂存区域**
	 - 将已经修改状态文件可以提交到暂存区域

### **Git的基本操作**
#### **配置用户信息** 
 - `git config --global user.name "runoob"` 
 - `git config --global user.email test@runoob.com`
 - 注意
	 - global设置后以后所有项目都默认使用此信息 
	 - 特殊的需要使用其他配置时去掉此选项重新配置即可
#### **初始化仓库**
 -  ``` mkdir learngit```
 -  ``` cd learngit``` 
 -  ```git init ```
 - 初始化完成后会生成.git文件夹（隐藏） 
	 - 查看使用命令  ls -ah命令查看

#### **添加文件到版本库**

 - 创建readme.txt
 - 添加内容 
 Git is a version control system. 
Git is free software.
 - **添加文件到暂存区**`git add readme.txt`
 - **提交文件到仓库**`git commit --m "wrote a readme file"`
	- --m 添加注释
 - git add 可一次性添加多个文件
``` git add file1.txt file2.txt```
```git commit -m "一次性可提交多个文件"```
 - 修改readme.txt内容
Git is a distributed version control system.
Git is free software.
 - **查看文件状态** `git status`
	> $ git status On branch master 
	> Changes not staged for commit:  
	>  (use "git add <file>..." to update what will be committed)   (use "git
	> checkout -- <file>..." to discard changes in working directory)
	> 
	>         modified:   readme.txt
	> 
	> no changes added to commit (use "git add" and/or "git commit -a")
	上面内容表示文件内容修改但没有提交到暂存区
	该文件标示为已经修改
	运行命令git diff readme.txt
 - **查看修改的内容** ```$ git diff readme.txt ```
	> diff --git a/readme.txt b/readme.txt index
	> d8036c1..6824adb 100644
	> --- a/readme.txt
	> +++ b/readme.txt @@ -1,2 +1,2 @@
	> -Git is a version control system.
	> +Git is a distributed control system.  Git is free software. \ No newline at end of file
 - 运行`git add readme.txt` 
 - 查看文件状态`git status`
	> $ git status On branch master Changes to be committed:   (use "git
	> reset HEAD <file>..." to unstage)
	> 
	>         modified:   readme.txt








  
 

