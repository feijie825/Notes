# 使用git的个人笔记    
> 克隆一个远程仓库命令：
> ~~~
> git clone git@github.com/rtx_blinky
> ~~~  
  
> 使用`.gitignore`时需要先建立该文件，然后再进行 commit 否则，需要忽略的文件将无法忽略。  
> 新建`.gitignore`时，先建立文本文档，然后另存为`.gitignore`。  
> ~~~
> --cached  贮存  
> -f        强制删除  
> ~~~
> 两个命令配合使用可保存本地文件的同时，删除仓库中指定文件。用于无需要跟的编译生成文件等。  
> `git rm --cached -f <filename>`    
# 使用git进行uVision工程管理  
1. 初始化中心仓库  
2. 从服务器克隆一个仓库  
3. 特性工作（暂存/提交/推送）  
4. 管理冲突  
## 版本控制下的工程文件  
> 在设置工作流之前，项目管理者需要清楚的知道哪些文件需要进行版本控制。当然所有的源代码文件需要进行版本控制。但是有一些文件特别针对于uVision需要被监视。
* 所有的用户生成源文件（*.c, *.cpp, *.h, *.inc, *.s)  
* 工作文件： Project.uvprojx（用于从头构建工程）  
* 工程选项文件：Project.uvoptx （包含关于高度器和追踪配置的信息）  
* 多项目工作区工程文件：Project.uvmpw  
* 复制到工程的实时环境配置文件（.\RTE下的所有文件）  
* 由软件组件创建的 #include列表：RTE\RTE_Components.h 文件  
* 器件配置文件：例如RTE\Device\LPC1857\RTE_Device.h  
* 手动创建的链接控制文件（Project.sct）  
* 所有相关的软件包文件（例如ARM::CMSIS, Keil::Middleware, Device Family Packs，等等）  
> 不需要被监视的文件  
* 工程屏幕布局文件：Project.uvguix.username  
* 作为Pack一部分的所有文件（完整的Pack将会被版本控制，并且可用于每个用户一旦使用Pack Installer进行安装）  
* 生成的子目录输出文件 .\Listings and .\Objects  
* 调试适配器的INI文件  
  
#集中式工作流示例  
1. 初始化中心仓库  
> 在命令行输入   
>~~~
>C:\workdir\RTX_Blinky>git init
>Initialized empty Git repository in C:/workdir/RTX_Blinky/.git/
>~~~  
现在，你已经添加你的远程仓库到你的工作拷贝。（请注意，这个远程仓库必须已经存在）  
~~~
C:\workdir\RTX_Blinky>git remote add origin https://github.com/account/rtx_blinky.git  
~~~
然后添加（暂存）工程的所有文件：  
C:\workdir\RTX_Blinky>git add *
warning: LF will be replaced by CRLF in Blinky.uvoptx.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in Blinky.uvprojx.
The file will have its original line endings in your working directory.  
> 注意：工程文件 uvprojx 和 uvoptx 有UNIX风格的行结束符（LF）。在 windows 系统中通常使用（CRLF)。Git 自动检测到这点，并且在服务器上将行结束符改变为 CRLF。但是稍后这两个工程文件的状态总是“已改变”。你可以绕过这个问题，使用命令git config --global core.autocrlf false  
提交所有文件，并为提交增加描述。  
~~~
C:\workdir\RTX_Blinky>git commit -m "Initial version"
[master (root-commit) b71a02e] Initial version
warning: LF will be replaced by CRLF in Blinky.uvoptx.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in Blinky.uvprojx.
The file will have its original line endings in your working directory.
12 files changed, 5408 insertions(+)
create mode 100644 Blinky.c
create mode 100644 Blinky.uvoptx
create mode 100644 Blinky.uvprojx
create mode 100644 Debug_RAM.ini
create mode 100644 Packs/ARM.CMSIS.4.3.0.pack
create mode 100644 Packs/Keil.LPC1800_DFP.2.4.0.pack
create mode 100644 RTE/CMSIS/RTX_Conf_CM.cApp Note 279 Copyright © 2015 ARM Ltd. All rights reserved
Using Git for Project Management with µVision 6 keil.com/appnotes/docs/apnt_279.asp
create mode 100644 RTE/Device/LPC1857/RTE_Device.h
create mode 100644 RTE/Device/LPC1857/startup_LPC18xx.s
create mode 100644 RTE/Device/LPC1857/system_LPC18xx.c
create mode 100644 RTE/RTE_Components.h  
~~~
最后向服务器推送文件。  
~~~
C:\workdir\RTX_Blinky>git push -u origin master
Username/Password
Counting objects: 19, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (17/17), done.
Writing objects: 100% (19/19), 101.31 MiB | 104.00 KiB/s, done.
Total 19 (delta 0), reused 0 (delta 0)
To https://github.com/account/rtx_blinky.git
* [new branch] master -> master
Branch master set up to track remote branch master from origin.
~~~  
> 注意：仅第一次推送时使用 `git push -u origin master` ，以后再进行推送时使用 `git push origin master` 即可。  
