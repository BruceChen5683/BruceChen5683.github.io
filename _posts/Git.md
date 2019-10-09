  git 如何查看两次提交之间有哪些文件有修改呢？

        git diff  HEAD HEAD^ --stat
        HEAD和HEAD^可以换成两次提交的版本号

  撤销最近一次已提交的修改
      git revert HEAD
      这样就创建了一个撤消了上次提交(HEAD)的新提交, 你就有机会来修改新提交(new commit)里的提交注释信息.
      
  重置“本地的”修改

			场景: 你在本地提交了一些东西（还没有 push），但是所有这些东西都很糟糕，你希望撤销前面的三次提交 — 就像它们从来没有发生过一样。

			方法: git reset <last good SHA> 或 git reset --hard <last good SHA>

  修改最近一次的提交log
        git commit --amend

  创建远程分支(本地分支push到远程)：$ git push origin [name]

  删除远程分支，tag
        git push origin --delete <branchName>

        git push origin :<branchName>  推送一个空分支到远程分支，其实就相当于删除远程分支：

        git push origin --delete tag <tagname>

        git tag -d <tagname>   git push origin :refs/tags/<tagname>   推送一个空tag到远程tag：

  删除不存在对应远程分支的本地分支
        git remote show origin

            e.g.
            * remote origin
            Fetch URL: git@github.com:xxx/xxx.git
            Push  URL: git@github.com:xxx/xxx.git
            HEAD branch: master
            Remote branches:
              master                 tracked
              refs/remotes/origin/b1 stale (use 'git remote prune' to remove)
            Local branch configured for 'git pull':
              master merges with remote master
            Local ref configured for 'git push':
              master pushes to master (up to date)

            这时候能够看到b1是stale的，使用 git remote prune origin 可以将其从本地版本库中去除。


        git fetch -p 更简单的方法是使用这个命令，它在fetch之后删除掉没有与远程分支对应的本地分支：

  删除本地分支   git branch -d xxxxx

      git br -d test   /  git branch -D  branch_a

  暂存修改
    git stash 将当前所有修改项(未提交的)暂存，压栈。此时代码回到你上一次的提交，用git status可查看状态。
    git stash list将列出所有暂存项。
    git stash clear 清除所有暂存项。
    git stash apply 将暂存的修改重新应用，使用git status可以看到以前暂存的修改又回来了

  git存储用户名与密码
      git config credential.helper store

      git config --global credential.helper cache
      ... which tells git to keep your password cached in memory for (by default) 15 minutes. You can set a longer timeout with:

      git config --global credential.helper "cache --timeout=3600"


      windows系统下:

      Finally, launch a command prompt and type:

      git config --global credential.helper winstore
      Or you can edit your .gitconfig file manually:

      [credential]
          helper = winstore
      git config --global credential.helper store


      如果出现git: 'credential-cache' is not a git command. See 'get --help'.-------------------------使用git config --global credential.helper wincred

  git 删除本地 untracked files          remove local (untracked) files from the current Git working tree?
        Step 1 is to show what will be deleted by using the -n option:
              git clean -n
        Clean Step - beware: this will delete files:
              git clean -f
  
  [git  push error](https://blog.csdn.net/viweei/article/details/6163562)
```
No refs in common and none specified; doing nothing.  
Perhaps you should specify a branch such as 'master'.  
fatal: The remote end hung up unexpectedly  

解决办法 git push origin master
```

- git init  和 git init --bare区别？？？


#####github创建私有库
https://github.com/BruceChen5683/Wiki


####版本回滚

#####本地分支版本回退
先用下面命令找到要回退的版本的commit id：
```
git reflog
```

接着回退版本:
```
git reset --hard commitId
```

#####自己的远程分支版本回退的方法
首先要回退本地分支：
```
reflog
git reset --hard Obfafd
```
紧接着强制推送到远程分支：
```
git push -f
```

#####公共远程分支版本回退的方法
使用git reset回退公共远程分支的版本后，需要其他所有人手动用远程master分支覆盖本地master分支，显然，这不是优雅的回退方法，下面我们使用另个一个命令来回退版本：
```
git revert HEAD                     //撤销最近一次提交
git revert HEAD~1                   //撤销上上次的提交，注意：数字从0开始
git revert 0ffaacc                  //撤销0ffaacc这次提交

revert 是撤销一次提交，所以后面的commit id是你需要回滚到的版本的前一次提交
使用revert HEAD是撤销最近的一次提交，如果你最近一次提交是用revert命令产生的，那么你再执行一次，就相当于撤销了上次的撤销操作，换句话说，你连续执行两次revert HEAD命令，就跟没执行是一样的
使用revert HEAD~1 表示撤销最近2次提交，这个数字是从0开始的，如果你之前撤销过产生了commi id，那么也会计算在内的。
如果使用 revert 撤销的不是最近一次提交，那么一定会有代码冲突，需要你合并代码，合并代码只需要把当前的代码全部去掉，保留之前版本的代码就可以了.
```

####使用git stash命令保存和恢复进度
```
git stash
     保存当前工作进度，会把暂存区和工作区的改动保存起来。执行完这个命令后，在运行git status命令，就会发现当前是一个干净的工作区，没有任何改动。使用git stash save 'message...'可以添加一些注释

git stash list
     显示保存进度的列表。也就意味着，git stash命令可以多次执行。

git stash pop [–index] [stash_id]
     git stash pop 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
     git stash pop --index 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
     git stash pop stash@{1}恢复指定的进度到工作区。stash_id是通过git stash list命令得到的 
     通过git stash pop命令恢复进度后，会删除当前进度。

git stash apply [–index] [stash_id]
     除了不删除恢复的进度之外，其余和git stash pop 命令一样。

git stash drop [stash_id]
     删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

git stash clear
     删除所有存储的进度。
```

- 忽略.idea
```
清除.idea的git缓存
git rm -r --cached .idea
```