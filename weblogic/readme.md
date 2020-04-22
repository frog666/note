## weblogic docker build

[github WeblogicEnvironment](https://github.com/QAX-A-Team/WeblogicEnvironment)

	docker build --build-arg JDK_PKG=jdk-7u21-linux-x64.tar.gz --build-arg WEBLOGIC_JAR=fmw_12.1.3.0.0_wls.jar  -t weblogic12013jdk7u21:v0 .


	docker run -d -p 7001:7001 -p 8453:8453 -p 5556:5556 --name weblogic12013jdk7u21 weblogic12013jdk7u21:v0


![](1.jpg)