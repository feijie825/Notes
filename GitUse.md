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