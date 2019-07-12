payload.txt 为 post 在和payload,**注意：payload这第一行的内容和间距不可修改**

Main.java 为 payload 加密 解密 脚本

index.jsp 为 payload 中 webshell部分


含有漏洞 页面
```ip/seeyon/htmlofficeservlet```

如果出现 ```DBSTEP V3.0     0               21              0               htmoffice operate err```，则可能有漏洞

构造post请求

请求头保留 POST，Host,User-Agent 三行字段

请求体为poc 内容

在burp中复现

Seeyon/A8/ApacheJetspeed/   log目录

漏洞版本 致远A8-V5协同管理软件V6.1sp1，V7.0sp1、V7.0sp2、V7.0sp3，V7.1

fofa搜索

```
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V6.1"     
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V6.1SP2"  
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V6.1SP1"  
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V7.0SP3"
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V7.0SP2"  
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V7.0SP1" 
app="用友-致远OA" && title=="致远A8-V5协同管理软件 V7.1"
```

