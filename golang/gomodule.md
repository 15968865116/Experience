Go语言使用go module
1、	go语言版本升级至1.11以上
a)	go语言版本下载，国内镜像网站：https://golang.google.cn/dl/
b)	使用wget https://golang.google.cn/dl/go1.15.5.linux-amd64.tar.gz 命令下载
c)	解压下载下来的文件
d)	将其复制到原来的go目录下。
e)	查询go版本  go version
2、	修改代理以及开启module
a)	export GO111MODULE=on   //开启module
b)	export GOPROXY=https://goproxy.io  //切换代理
3、	应用vs扩展
a)	按上述情况改变代理后，使用go get -v golang.org/x/tools/gopls  安装官方的语言服务器
在vs中直接使用会报错，使用go get在终端直接安装
4、	Go module使用
a)	在gopath以外创建文件夹
b)	创建文件时使用 go mod init 文件夹名 初始化文件夹，使生成mod文件，mod文件就是项目所需要的各种包名
c)	添加了新的包后 使用命令  go mod tidy 进行包的添加（目前发现在vs中不能很好地使用go get 来获取包）
	
