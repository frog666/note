https://github.com/vzex/dog-tunnel

## 安装


安装 go

利用 go get命令一键获取代码、编译并安装

安装依赖：

	go get github.com/go-sql-driver/mysql
	
	go get github.com/klauspost/reedsolomon
	
	go get github.com/cznic/zappy

编译程序 

	go get -u -d github.com/vzex/dog-tunnel && cd $GOPATH/src/github.com/vzex/dog-tunnel/ && git checkout master && make

默认路径在 ```/root/go/src/github.com```


也可以直接下载release版本(release 版本不支持P2P模式)


