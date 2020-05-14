---
layout: mypost
title: manjaro安装mariaDB
categories: [linux]
---

### mariaDB
	是mysql的一个分支，在已存在mysql的情景下可以进行无缝衔接。
### 安装

- 更换源.这个命令执行后.会出现弹窗.弹出的是对国内源的排序

```
pacman-mirrors -i -c China -m rank
```

- 安装mariaDB

```
sudo pacman -S mariadb
```

- 让本地的包数据库和远程的软件仓库同步

```
sudo pacman -Sy 
```

- 初始化

```
sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

执行完后出现下方信息

```
Installing MariaDB/MySQL system tables in '/var/lib/mysql' ...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system


PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:

'/usr/bin/mysqladmin' -u root password 'new-password'
'/usr/bin/mysqladmin' -u root -h ltt-pc password 'new-password'

Alternatively you can run:
'/usr/bin/mysql_secure_installation'

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the MariaDB Knowledgebase at http://mariadb.com/kb or the
MySQL manual for more instructions.

You can start the MariaDB daemon with:
cd '/usr' ; /usr/bin/mysqld_safe --datadir='/var/lib/mysql'

You can test the MariaDB daemon with mysql-test-run.pl
cd '/usr/mysql-test' ; perl mysql-test-run.pl

Please report any problems at http://mariadb.org/jira

The latest information about MariaDB is available at http://mariadb.org/.
You can find additional information about the MySQL part at:
http://dev.mysql.com
Consider joining MariaDB's strong and vibrant community:
https://mariadb.org/get-involved/

```

- 启动mysql服务

```
systemctl start mysqld.service
```

- 启动服务器后输入如下命令进行数据库设置

```
sudo /usr/bin/mysql_secure_installation
```

出现下方信息，询问是否使用root账户密码是否和系统管理员账户密码一致，Enter即可

```
Enter current password for root (enter for none):
```

```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] n
 ... skipping.

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

- 使用
```
mysql -u root -p
```

#### mysql服务设置命令

##### 停止mysql服务

```
systemctl stop mysqld.service
```

##### 重启mysql服务

```
systemctl restart mysqld.service
```

##### 查看mysql当前状态

```
systemctl status mysqld.service
```

##### 设置mysql服务开机自启动

```
systemctl enable mysqld.service  
```

##### 停止mysql服务开机自启动

```
systemctl disable mysqld.service  
```

#### 赋予root账户远程连接权限

```
mysql -u root -p

grant all privileges on *.* to 'root'@'%' identified by '123456';

flush privileges;
```

### 参考
- [Manjaro 安装 mariadb 数据库 (命令行安装)](https://blog.csdn.net/qq_40451749/article/details/82288688)
- [ManjaroLinux安装Mariadb(即mysql)](https://blog.csdn.net/tiandc/article/details/89607756)
