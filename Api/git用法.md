# git常用命令速查表

> 基本配置

#### 1.初始化 `git init`

#### 2.配置邮件`git config --global user.name "xxx" `

#### 查看当前用户名 `git config user.name`

#### 3.配置用户名`git config --global user.email "xxx@xxx.com"  `

#### 查看当前用户名 `git config user.email`

#### 4.查看仓库状态 `git status`

> 增删改查

#### 1. `git add .` 把当前目录下所有文件添加到暂存区

#### 2.`git commit -m "说明"`，添加说明，这次更改做了什么

#### 3.`git ls-files `看已经被提交的

#### 4.`git log` 查看所有版本更改记录

#### 5.`git log --oneline` 查看所有版本更改记录并且每个版本一行显示

#### 6.`git log -p` 查看所有版本更改记录和更改内容

#### 7.`git checkout id`回到某个版本

#### 8.`git tag -a 版本名称 -m "版本描述"` 添加版本号(添加-a属性才可以添加-m属性)

#### 9.`git tag` 查看当前所有版本号

#### 10.`git log --oneline` 查看所有版本记录（会出现版本号）

#### 11.`git tag -a 版本号 版本id` 添加版本号

#### 12.`git rm 文件名(包括路径)` 从git中删除指定文件

#### 13.`git rm [file name]` 删除一个文件

> 分支

#### 1.`git branch` 查看本地分支

#### `git branch -f`查看远程分支

#### 2.`git checkout 版本id` 退回到某个要修改的版本

#### 3.`git branch 分支名` 新建分支

#### 4.`git checkout 分支名` 移动到分支2中

#### 5.`git checkout 原分支` 回到原分支中

#### 6.`git log --online --graph --all` 查看版本记录图形结构

#### 7.`git merge 分支2` 合并分支2

####8.`git branch -d 分支名` 删除本地分支名

####9.`git push origin --delete 分支名` 删除远程分支名

> 远程

#### 1.`git clone 地址` 克隆项目到本地

#### 2.`git remote add  github`  添加远程名称，默认origin

#### 3.`git remote -v` 列出所有远程名称的详细信息

#### 4.`git pull` 本地与服务器端同步

#### 5.`git push -u github master`上传代码到远程仓库