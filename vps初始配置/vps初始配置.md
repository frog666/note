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