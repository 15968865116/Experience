# 配置golang 1.15 + VScode （在Ubuntu下）

# golang配置

## 下载安装包

```bash
wget https://golang.google.cn/dl/go1.15.2.linux-amd64.tar.gz
```

## 解压

```bash
sudo tar -C /usr/local -xzf go1.15.2.linux-amd64.tar.gz
```

## 设置环境变量

```bsah
echo 'export PATH=$PATH:/usr/local/go/bin' >> .profile && . .profile
```

## 检查安装情况

```bash
go version
```

## 配置环境

设置环境变量
在 /etc/bash.bashrc 文件中添加如下内容(重启命令行后生效)：

打开文件

```bash
sudo gedit /etc/bash.bashrc
```

```bash
export GOROOT_BOOTSTRAP=/usr/local/go
export GOROOT=/usr/local/go
export GOPATH=/usr/local/gopath
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

重启命令行

```bash
source /etc/bash.bashrc 
```



# VScode配置

## 官网下载Ubuntu版本的安装包

[官网链接](https://code.visualstudio.com/)

选择：code_1.52.1-1608136922_amd64.deb

## 安装

```bash
sudo dpkg -i code_1.52.1-1608136922_amd64.deb
```

## 配置go module 和 go 代理

开启go module 和 go 代理后 vscode 才可以顺利安装一些插件

```bash
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```
