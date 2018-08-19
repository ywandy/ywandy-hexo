---
title: fabric的编译安装
date: 2018-08-16 11:57:18
tags:
    - fabric
    - hyperledger
    - 区块链
    - go
---
# 安装go

可以参照我上一篇博客
```bash
wget https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz
tar -xvf go1.10.3.linux-amd64.tar.gz
mv go /usr/local/
mkdir -p ~/go  
vim /etc/profile
export GOPATH=~/work
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
source /etc/profile
```

# 安装依赖包

```bash
apt-get update
apt-get install -y libsnappy-dev zlib1g-dev libbz2-dev libltdl-dev libtool
```

# Fabric源码下载

```bash
git clone https://github.com/hyperledger/fabric $GOPATH/src/github.com/hyperledger/fabric
git clone https://github.com/hyperledger/fabric-ca $GOPATH/src/github.com/hyperledger/fabric-ca
```
或
```bash
go get github.com/hyperledger/fabric
go get github.com/hyperledger/fabric-ca
```

# 编译fabric代码
```bash
 cd $GOPATH/src/github.com/hyperledger/fabric
 make native
 make docker
 cp build/bin/* $GOPATH/bin/
```

# 解决编译fabric过程中报错
#### 1. 解决make docker时候报错golang-x-tool错误:

错误信息：

```bash
package golang.org/x/tools/go/gcexportdata: unrecognized import path "golang.org/x/tools/go/gcexportdata"
```
解决方案
```bash
git clone https://github.com/golang/tools.git $GOPATH/src/golang.org/x/tool
```
#### 2. 解决：cp: cannot stat ‘build/docker/gotools/bin/protoc-gen-go’: No such file or directory 

```bash
go get github.com/golang/protobuf/protoc-gen-go
cd $GOPATH/src/github.com/golang/protobuf/
make all
cd $GOPATH/src/github.com/hyperledger/fabric
cp $GOPATH/bin/protoc-gen-go .build/docker/gotools/bin/
```

# 编译fabric-ca代码

``` bash
cd $GOPATH/src/github.com/hyperledger/fabric-ca
make docker

cd $GOPATH/src/github.com/hyperledger/fabric
make release
make docker
cp .build/bin/* $GOPATH/bin/
```

 
