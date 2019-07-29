## 设置用户　
* $ git config --global user.name "Your Name"
* $ git config --global user.email "email@example.com"


## 设置根目录 
* $ git init

首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。

## 提交修改到Git仓库(分两步)
* 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
* 第二步，使用命令git commit　－m 变更说明，完成。

## 查看仓库/文件当前状态(difference)
* git status
* git diff 文件名/仓库名

##　查看提交日志
* git log
* git log --pretty=oneline
* git reflog
记录每一次命令

```
说明:
3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file

第一串数字,形如3628164...882e1e0的即是commit id（版本号）
```

## 版本回退
在Git中，用HEAD表示当前版本，HEAD^表示上一个版本，Header^^表示上上一个版本

* git reset --hard HEAD^  
回退到上一版本
* git reset --hard 文件版本号
回退到某一个版本

##　版本库和暂存区(stage)
* 版本库:工作区中隐藏目录.git
* 暂存区:git一个类似缓冲区的存在
	* 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
	* 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。


## 管理修改
* git checkout -- filename
文件回到最近一次git commit或git add时的状态。

	* 如果filenam自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
	* 如果filename已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

## 远程仓库操作
克隆时,必须知道仓库的地址

* git clone repository-address
克隆远程库
* git remote add `repository-name` `repository-address` 
关联远程库
* git push origin `branch-name` 
推送本地分支到远程库
* git pull origin `branch-name`
拉远程分支到本地

## 分支操作
* git checkout -b dev
创建并切换到新分支
* git branch dev
创建分支
* git checkout dev
切换分支
* git branch dev 
* git merge dev
* git branch -D dev

## 暂存修改
* git stash
* git stash list
* git stash apply : 恢复但不删除  
* git stash pop   : 恢复并删除
* git stash drop  : 删除

## 提取修改
* git show `commit-id` `filename`
很实用



## 删除
* git branch -D `branch-name` : 删除本地分支
* git push origin :`branch-name` : 删除远程分支(省略了本地分支名,相当推送空分支到远程)

## 标签管理
* git tag : 显示已有标签
* git tag -l 'pattern' : 过滤显示指定模式的标签 
* git tag -a `tag-name` -m `tag-message` :创建标签
* git show :查看标签的版本信息
* git tag -a `tag-name` `commit-id` :后期打标签
* git push origin `tag-name` : 推送标签到远程,默认不会推送
* git tag -d `tag-name` : 删除标签
* git push origin :`branch-name/tags/tag-name` : 删除远程标签





小结
  ● 搭建Git服务器非常简单，通常10分钟即可完成；
  ● 要方便管理公钥，用Gitosis；
  ● 要像SVN那样变态地控制权限，用Gitolite。
  
  


# PATCH 1

## 常用命令
  * git status -s/--short
  * git diff --staged
	查看暂存区内的修改
  * git log -p 
  * git log --stat
  * git log --pretty


## .gitignore 规范
  * 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
  * 可以使用标准的 glob 模式匹配。
  * 匹配模式可以以（/）开头防止递归。
  * 匹配模式可以以（/）结尾指定目录。
  * 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

## 冲突工具
  *  git mergetool





























