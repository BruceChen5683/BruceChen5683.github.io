1. repo init error
```
Get https://gerrit.googlesource.com/git-repo/clone.bundle
Get https://gerrit.googlesource.com/git-repo
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

2.  查看和切换分支
```
在AOSP目录下，我们可以用repo来查看当前所有可切换的分支，输入
cd .repo/manifests
git branch -a | cut -d / -f 3
可以列出所有的分支
从中选择android-8.1.0_r7，进行切换，切换的命令如下
repo init -b android-8.1.0_r7
repo sync
```

3. 下载
```
https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/
```