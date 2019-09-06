centos 7 初始配置


安装gcc

yum -y install gcc

安装zlib依赖

yum install -y zlib zlib-devel --setopt=protected_multilib=false


安装zlib

yum -y install zlib*


下载python3 并make install

建立软连接

ln -s /usr/local/python3/bin/python3 /usr/bin/python3

查看版本

python3 -V