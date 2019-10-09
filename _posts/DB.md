### SQL

####1. MySql
1. Navicat连接不上 1251 client does no support authentic  **解决方法是将root的plugin改成mysql_native_password。相当于降了一级。**
```
客户端使用navicat for mysql。本地安装了mysql 8.0。但是在链接的时候提示：

主要原因是mysql服务器要求的认证插件版本与客户端不一致造成的。

打开mysql命令行输入如下命令查看，系统用户对应的认证插件：

可以看到root用户使用的plugin是caching_sha2_password，mysql官方网站有如下说明：

意思是说caching_sha2_password是8.0默认的认证插件，必须使用支持此插件的客户端版本。

plugin的作用之一就是处理后的密码格式和长度是不一样的，类似于使用MD5加密和使用base64加密一样对于同一个密码处理后的格式是不一样的。

解决方法：

我不希望更新本地的客户端版本，想直接使用原来的环境来链接。

解决方法是将root的plugin改成mysql_native_password。相当于降了一级。

mysql官方网站提供了从mysql_old_password升级到mysql_native_password，我们可以仿照这个。官方原文如下：

这里改成：

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';

这行代码有两层含义，第一:修改root的密码为'root'，摒弃原来的旧密码。第二：使用mysql_native_password对新密码进行编码。

修改完成后再用客户端登陆成功：

补充：如果在修改插件的时候出现错误，可现将插件改为 mysql_old_password，然后再升级成mysql_native_password，方法：
```

2. mysql 查看端口号
```
mysql> show global variables like 'port';
```

3. MySQL ERROR 1698 (28000) 错误  **错误的起因就是在这里， root的plugin被修改成了auth_socket，用密码登陆的plugin应该是mysql_native_password。**
```
mysql> select user, plugin from mysql.user;
+-----------+-----------------------+
| user      | plugin                |
+-----------+-----------------------+
| root      | auth_socket           |
| mysql.sys | mysql_native_password |
| dev       | mysql_native_password |
+-----------+-----------------------+
3 rows in set (0.01 sec)

mysql> update mysql.user set authentication_string=PASSWORD('newPwd'), plugin='mysql_native_password' where user='root';
Query OK, 1 row affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 1

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

sudo service mysql stop
sudo service mysql start
```