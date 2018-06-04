# git config
    core.autocrlf
    core.filemode

# git config --global credential.helper cache

# git reflog

# git clean -fdx

# git status

# git show

# git fetch origin

# git cherry-pick

# git log -p
# git log --stat
# git log --oneline
# git log --grep=<pattern>
# git log --before='2017-01-01'

# git shortlog

# git 迁移到 git

条件：本地有一个clone的仓库，用gitlab搭建的Git server。

1. 在gitlab上建立一个新的项目，设其地址为http://192.168.1.200/$user/$project.git

1. 进入本地的$project目录，然后执行下面两个命令：
    git remote remove origin

    git remote add origin http://192.168.1.200/$user/$project.git

    然后就可以用$user这个gitlab上的账号进行pull和push了。

# git 修改目录
### windows
因为不区分大小写，命令识别为迁移。如下可成功修改目录。
    git mv foldername tempname && git mv tempname folderName (在大小写不敏感的系统中，如windows，重命名文件的大小写,使用临时文件名)

