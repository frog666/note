centos 7 初始配置

查看 vps 信息

cat /etc/os-release


安装gcc

yum -y install gcc

安装zlib依赖

yum install -y zlib zlib-devel --setopt=protected_multilib=false


安装zlib

yum -y install zlib*

yum -y install python3

下载python3 并make install

建立软连接

ln -s /usr/local/python3/bin/python3 /usr/bin/python3

查看版本

python3 -V


安装jdk 1.8

yum search java |grep jdk|grep java-1.8.0


yum install -y java-1.8.0-openjdk

安装 apache 服务

yum install httpd -y

启动apache服务

	 systemctl start httpd.service

查看apache状态
	
	 systemctl status httpd.service

在浏览器输入ip访问，如果无法访问，在防火墙添加规则

centos7 默认预装 firewall 防火墙；

您可输入命令： systemctl status firewalld 查看状态

firewall-cmd --list-port 查看已放通的端口

另外 在控制台还有安全组 限制，如果需要访问某端口，在防火墙开启的状态下 ，设置规则，还需在安全组 入方向添加对应的端口，才可以正常访问

systemctl status firewalld

systemctl start firewalld


	firewall-cmd --list-all

添加入站端口规则

	firewall-cmd --zone=public --add-port=80/tcp --permanent

不关闭服务重载防火墙

	firewall-cmd --reload

如需关闭入站端口规则
	
	firewall-cmd --zone=public --remove-port=80/tcp --permanent

发现还是不行，问华为云客服，客服说  安全组的设置只能在console 界面修改，这一点腾讯云做的就不错。因为有时候我可能手机连terminal，登陆网页console太麻烦

![](2.jpg)

<br/>

![](1.jpg)

用新买的华为云服务器查看开放端口，发现默认开启了一个postfix邮件服务器，客服居然说默认不开放！腾讯云服务器没有默认开启这个服务

把服务停掉

	service postfix stop


安装mysql (我这里装的mysql5.7)

在CentOS7中默认安装有MariaDB，这个是MySQL的分支，但为了需要，还是要在系统中安装MySQL，而且安装完成之后可以直接覆盖掉MariaDB。

	wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
	yum -y install mysql57-community-release-el7-10.noarch.rpm
	yum -y install mysql-community-server

![](3.jpg)

我的腾讯云服务器还装了一个 php-mysql-5.4.16-46.el7.x86_64
 

启动mysql

systemctl start  mysqld.service

查看mysql状态

systemctl status mysqld.service

查看mysql密码，可生成一个临时密码

	grep "password" /var/log/mysqld.log

![](4.jpg)

用查到的密码登陆

修改密码(因为有密码策略，所以改一个稍微复杂的)

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';

查看密码规则

SHOW VARIABLES LIKE 'validate_password%';

![](5.jpg)


可以修改密码规则这样可以设置一些简单密码

	mysql> set global validate_password_policy=0;


	mysql> set global validate_password_length=1;


卸载 rpm 防止 mysql yum update时自动更新mysql

yum -y remove mysql57-community-release-el7-10.noarch

查看mysql 用户 登陆用户名 host

	use mysql;

![](6.jpg)

添加一个新用户给远程连接权限；给特定数据库的 增，改，查权限。

创建新用户 ('%'表示允许远程连接)

	CREATE USER 'a'@'%' IDENTIFIED BY '1';

![](7.jpg)

未分配权限 默认可以查看 information_schema 数据库所有表。

![](8.jpg)

分配用户权限参考 

https://blog.csdn.net/jasonzds/article/details/78117178

查看mysql 写本地文件路径

SELECT @@global.secure_file_priv; 或 SHOW VARIABLES LIKE "secure_file_priv";

![](9.jpg)

一般默认都是这个路径。如果为NULL，即不允许导入导出。

或者直接查看完整的

![](10.jpg)

修复方法：

windows下：修改my.ini 在[mysqld]内加入secure_file_priv =

linux下：修改my.cnf 在[mysqld]内加入secure_file_priv =

安装php

CentOs 7.X 添加源

	rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
	rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

安装php （5.6）,如果想升级到7.0把上面的56w换成70w就可以了。


yum install php56w.x86_64 php56w-cli.x86_64 php56w-common.x86_64 php56w-gd.x86_64 php56w-ldap.x86_64 php56w-mbstring.x86_64 php56w-mcrypt.x86_64 php56w-mysql.x86_64 php56w-pdo.x86_64

安装PHP FPM   (如果想升级到7.0把上面的56w换成70w就可以了。)

yum install php56w-fpm   


添加apache php 支持  （参考 https://www.cnblogs.com/yulibostu/articles/10560904.html）

	vim /etc/httpd/conf/httpd.conf

添加

	LoadModule php5_module modules/libphp5.so

在这```<IfModule mime_module>```添加

	<IfModule mime_module>

		AddType application/x-httpd-php .php
		AddType applicaiton/x-httpd-php-source .phps

