sudo apt-get install aspell graphviz php7.0-curl php7.0-gd php7.0-intl php7.0-ldap php7.0-mysql php7.0-pspell php7.0-xml php7.0-xmlrpc php7.0-zip


default_storage_engine = innodb
innodb_file_per_table = 1
innodb_file_format = Barracuda

登录后，您将看到mysql>提示。运行以下命令以创建数据库：

CREATE DATABASE moodle DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
然后创建一个Moodle用户，这样我们就不必告诉Moodle应用程序我们的root密码了。执行以下命令：

注意：在接下来的两个命令中，moodler使用您的Moodle用户名和moodlerpassword所选密码替换。

create user 'moodler'@'localhost' IDENTIFIED BY 'moodlerpassword';
并授予moodler用户编辑数据库的权限。该用户需要创建表并更改权限：

GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodler'@'localhost' IDENTIFIED BY 'moodlerpassword';
现在退出MqSQL命令行界面：

quit;