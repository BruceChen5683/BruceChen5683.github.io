1. find
```
find .|grep -ri "待查询字符串"      当前路径

$ find . -name 'my*'
搜索当前目录（含子目录，以下同）中，所有文件名以my开头的文件。

$ find . -name 'my*' -ls
搜索当前目录中，所有文件名以my开头的文件，并显示它们的详细信息。

$ find . -type f -mmin -10
搜索当前目录中，所有过去10分钟中更新过的普通文件。如果不加-type f参数，则搜索普通文件+特殊文件+目录。
```

2. 源的增减
```
  sudo add-apt-repository ppa:eugenesan/java
  添加好更新一下： sudo apt-get update

  进入 /etc/apt/sources.list.d 目录，将相应 ppa 源的保存文件删除。
```

3. 查看当前系统多少位
```
getconf LONG_BIT 
```

4. ssh
```
安装 
ssh localhost    
ssh: connect to host localhost port 22: Connection refused    
ssh localhost
ssh: connect to host localhost port 22: Connection refused如上所示，表示没有还没有安装，可以通过apt安装，命令如下：
***** sudo apt-get install openssh-server  OK*****
在linux下一般用scp这个命令来通过ssh传输文件。


1、从服务器上下载文件
scp username@servername:/path/filename /var/www/local_dir（本地目录）

 例如scp root@192.168.0.101:/var/www/test.txt  把192.168.0.101上的/var/www/test.txt 的文件下载到/var/www/local_dir（本地目录）


2、上传本地文件到服务器
scp /path/filename username@servername:/path

例如scp /var/www/test.php  root@192.168.0.101:/var/www/  把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中



3、从服务器下载整个目录
scp -r username@servername:/var/www/remote_dir/（远程目录） /var/www/local_dir（本地目录）

例如:scp -r root@192.168.0.101:/var/www/test  /var/www/

4、上传目录到服务器
scp  -r local_dir username@servername:remote_dir
例如：scp -r test  root@192.168.0.101:/var/www/   把当前目录下的test目录上传到服务器的/var/www/ 目录


文件传输
  scp -P 22 -r root@192.168.1.1:/root /g  复制远程目录到本地
```

5. evn 查找程序
```
#!/usr/bin/env python
```

6. #####fork机制？

7. passwd更新密码
```
passwd usrname
```

8. ###FAQ
```
解决ssh_exchange_identification:read connection reset by peer 原因
  可以通过ssh -v查看连接时详情
    vi /etc/hosts.allow
    sshd: ALL
    service sshd restart

-----------------------------------------------------------------------------------------------------------------------
make: Nothing to be done for `all' 解决方法


	make: Nothing to be done for `all' 解决方法
	1.这句提示是说明你已经编译好了，而且没有对代码进行任何改动。
	
	若想重新编译，可以先删除以前编译产生的目标文件：
	make clean
	然后再
	make
	 
	
	2.出现这种情况解决方法：
	
	
	a.make clean 清除安装时留下的文件
	
	b.在运行一下ldconfig


```

9. wget
```
wget -c 续传  status 206 200
```

10. tar
```
对于tar版本大于1.22（http://www.gnu.org/software/tar/），直接一条命令tar -xvf balabala.tar.xz就足矣，因为从1.15起tar就可以自动识别压缩的格式。
```

11. ubuntu16.04 编译OpenJDK 
```
重点参考https://blog.csdn.net/leonliu06/article/details/78495035
部分依赖安装失败,使用aptitude尝试
 2317  sudo apt-get install libmotif3
 2318  sudo apt-get install aptitude 
 2319  sudo aptitude install build essential gawk m4 libasound2-dev libcups2-dev libxrender-dev xorg-dev xutils-dev x11proto-print-dev binutils libmotif3 libmotif-dev ant  

使用su
ubuntu默认安装的是openJdk，需要使用oracle的jdk6u45作为bootsrap jdk,即oracle的jdk路径写入ALT_BOOTDIR

修改日期时间配置文件
jdk/src/share/classes/java/util/CurrencyData.properties
535行
TR=TRL;2004-12-31-22-00-00;TRY
修改为当前年份的10年内，eg:
今年2016，那么修改为
TR=TRL;2015-12-31-22-00-00;TRY
这个是根据地区码定的时间，所以最好是把此文件中所有涉及的时间日期按上面的规则修改。


修改虚拟机make文件，这个会导致编译jvm失败，报Err 2，不支持的操作系统，原因是部支持当前linux内核版本
hotspot/make/linux/Makefile
234行
SUPPORTED_OS_VERSION = 2.4% 2.5% 2.6% 3%
为
SUPPORTED_OS_VERSION = 2.4% 2.5% 2.6% 3% 4%


 [javac] /home/jhj/code/java/source_code/openjdk/langtools/src/share/classes/com/sun/tools/javac/comp

/Resolve.java:2182: warning: [overrides] Class Resolve.InapplicableSymbolsError.

Candidate overrides equals, but neither it nor any superclass overrides hashCode method
    [javac]         private class Candidate {
    [javac]                 ^   
    [javac] error: warnings found and -Werror specified
    [javac] 1 error
    [javac] 1 warning

有时候编译OpenJDK7会出现
ERROR: echo "*** This OS is not supported:" 'uname -a'; exit 1;  

这时我们编译时用下面的命令
make DISABLE_HOTSPOT_OS_VERSION_CHECK=OK  


如果出现下面的错误
Error: JAVA_HOME is not defined correctly.  
We cannot execute /NO_BOOTDIR/bin/java  
这个没有发现ALT_BOOTDIR，虽然build.sh已经设置了，我们在编译命令中加入这个

make ALT_BOOTDIR=/usr/lib/jvm/jdk1.7.0_80 DISABLE_HOT  
SPOT_OS_VERSION_CHECK=OK  

ERROR: libjvm.so: undefined reference to `void G1SATBCardTableModRefBS::write_ref_array_pre_work
template <class T> void write_ref_array_pre_work(T* dst, int count);

/* 
virtual void write_ref_array_pre(oop* dst, int count, bool dest_uninitialized) {
    if (!dest_uninitialized) {
      write_ref_array_pre_work(dst, count);
    }
}
virtual void write_ref_array_pre(narrowOop* dst, int count, bool dest_uninitialized) {
    if (!dest_uninitialized) {
      write_ref_array_pre_work(dst, count);
    }
}
*/

```
12. g++: internal compiler error: Killed (program cc1plus)解决方法

```
g++: internal compiler error: Killed (program cc1plus)解决方法
    原因是系统内存不足，没有交换分区, 编译过程中内存耗尽, 导致了编译中断 …解决方式也很简单, 就是（临时）增加一个交换分区:
        sudo dd if=/dev/zero of=/swapfile bs=64M count=16   创建分区文件, 大小 64M*16
        sudo mkswap /swapfile  生成 swap 文件系统
        sudo swapon /swapfile  激活 swap 文件

        After compiling, you may wish to
        Code:

        sudo swapoff /swapfile
        sudo rm /swapfile

             swapon /swapfile
             这样就木有问题了, 但是这样并不能在系统重启的时候自动挂载交换分区, 这样我们就需要修改 fstab.
             修改 /etc/fstab 文件, 新增如下内容:

             /swapfile  swap  swap    defaults 0 0
             这样每次重启系统的时候就会自动加载 swap 文件了.

             备注：1. 创建的交换分区大小为： bs x count。

                  2. Swap 空间的作用可简单描述为：当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前                          运行的程序使用。那些被释放的空间可能来自一 些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间（磁盘空间虚拟成内存使用）中，等到那些程序要运行时，再从Swap中恢复保存 的数据到内存中。
```


13.  pip Import Error:cannot import name main
```
后来发现是因为将pip更新为10.0.0后库里面的函数有所变动造成这个问题。 
解决方案：
sudo gedit /usr/bin/pip
将原来的：

from pip import main
if __name__ == '__main__':
    sys.exit(main())
改成：

from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```

14. 'hgfs' protocol error
```
**关闭(一定要先关闭)**VM上的系统（对Ubuntu进行“虚拟机设置”前必须先关闭Ubuntu，否则会出错），右键点击虚拟机，打开”虚拟机设置（settings）”，切换到”选项（options）”标签，点击”共享文件夹（shared folders）”，默认是禁用状态，鼠标选中此项，在右边的”文件夹共享(folder sharing)”中，选择”总是启用(always enabled)“，可以在下面的方框中选择”添加（add）”，选择需要在Linux中共享的win下文件夹。最后点击”确定“即可。 
再启动VM里的系统，刚才的错误提示没有了。并且可以通过 ls /mnt/hgfs命令，看到你刚刚共享的文件夹，在linux系统中就可以运用这个文件夹内的内容了。
```

15. 版本查看
```
uname -a 
sudo lsb_release -a
cat /etc/issue
```

16. curl
```
curl -o out http://www.baidu.com 下载网站内容保存到out文件
```

17. rsync sync

```
rsync -P -a xxx@0.0.0.0:/data/src /home/battlecall/copyhere
```

