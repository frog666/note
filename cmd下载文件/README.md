js和certutil,bitsadmin,vbs



识别英文操作系统名称及版本   ```systeminfo | findstr /B /C:"OS Name" /C:"OS Version"```

别中文操作系统名称及版本     ```systeminfo | findstr /B /C:"OS 名称" /C:"OS 版本"``` 

                            ```
                            OS 名称:          Microsoft Windows 10 旗舰版
                            OS 版本:          10.0.14393 N/A Build 14393
                            ```

识别系统体系结构            ```echo %PROCESSOR_ARCHITECTURE% ```

                           ```AMD64```
                           
查看某特定用户信息          ```net user XXX```

查看路由表信息route print

查看ARP缓存信息 arp -A 

查看防火墙规则：

``` netstat -ano ```
``` netsh firewall show config ```
```netsh firewall show state ```

具体参考[Windows内网渗透提权的几个实用命令](https://www.freebuf.com/articles/system/114731.html)

https://www.jianshu.com/p/502441bcebe0   15种下载

win7测试下载https会报错，提醒我丢失libeay_32.dll，libeay32.dll是OpenSSL所用动态链接库

```perl -MLWP::Simple -e "getstore ('http://www.163.com/','163.html')"```

测试失败 ```perl -MLWP::Simple -e "getstore ('https://raw.githubusercontent.com/3gstudent/test/master/version.txt','163.txt')"```

```perl -e "print 'Hello World \n'"```   

downloader常用方法如下：

certUtil
powershell
csc
vbs
JScript
hta
bitsadmin
wget
debug
ftp
ftfp


python cmd 下载文件

```python -c "import urllib2;u=urllib2.urlopen('https://raw.githubusercontent.com/3gstudent/test/master/version.txt');localfile=open('a.txt','w');localfile.write(u.read());localfile.close();"```


httpdownload.rb 可用ruby 下载 http链接的文件，https暂不知道下载方法  ```$ruby httpdownload.rb ```
