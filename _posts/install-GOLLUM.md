结合[gollum](https://github.com/gollum/gollum)　　　Windows	JRuby (1.9.3+ compatible)	RJGit	almost1

1. 安装[git for windows](https://git-for-windows.github.io/)

2. 安装[JRuby](http://jruby.org/)

3. 安装rjgit　　　***gem install rjgit***
```
    Successfully installed rjgit-4.8.0.0
    1 gem installed
```

4. 安装gollum　　　***gem install gollum***

5. 创建wiki存储目录　　　***mkdir  wiki_project***

6. wiki_project仓库初始化　　　***cd wiki_project,git init***

7. 启动gollum
```
[2017-11-03 17:16:04] INFO  WEBrick 1.3.1
[2017-11-03 17:16:04] INFO  ruby 2.3.3 (2017-09-06) [java]
== Sinatra (v1.4.8) has taken the stage on 4567 for development with backup from WEBrick
[2017-11-03 17:16:04] INFO  WEBrick::HTTPServer#start: pid=27168 port=4567
```
8. 访问[本地所有wiki项目](http://localhost:4567/pages)

9. _**中文支持**_  安装gollum-rugged_adapter, 运行gollum时, **加上参数 --adapter 为 rugged**
```
sudo apt-get -y install cmake  或yum install
sudo gem install gollum-rugged_adapter
gollum --port 80 --adapter rugged
```

qvbg9:NsTq8eVNb
