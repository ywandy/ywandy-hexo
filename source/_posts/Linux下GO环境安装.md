---
title: Linux下GO环境安装
date: 2018-08-16 11:59:29
tags:
    - go
    - 环境搭建
    - linux
categories: 
    - 环境安装
---
## Linux 下GO 环境安装:

### 安装环境:

1. 从Golang DL下载go1.10.3.linux-amd64.tar.gz

   ```wget https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz```

2. 解压`go1.10.3.linux-amd64.tar.gz` 

   ```tar zxvf go1.10.3.linux-amd64.tar.gz```

3. 移动go到/usr/local

   ```mv go /usr/local```

4. 在`~/.profile`加入环境变量:

   ```
   export PATH=$PATH:/usr/local/go/bin
   export PATH=$PATH:$GOROOT/bin
   ```

5. 在家目录下建立go目录:

   `mkdir -p ~/go `  

6. 创建一个工程路径（GOPATH)例如我在家目录创建work路径：

   `mkdir ~/work -p`

7. 加入`~/work`到.profile环境变量：

   ```export GOPATH=~/work```  

8. 使.profile生效:

   `source ~/.profile`

   

### 测试环境:

1. 在命令行打入`go version`可以看到类似:

   ```bash
   # yewei @ xinou-work in ~/yyw/fabric/work [9:58:07]
   $ go version
   go version go1.10.3 linux/amd64
   ```

2. 在命令行打入`go env`可以看到类似:

   ```bash
   # yewei @ xinou-work in ~/yyw/fabric/work [10:00:50]
   $ go env
   GOARCH="amd64"
   GOBIN=""
   GOCACHE="/home/yewei/.cache/go-build"
   GOEXE=""
   GOHOSTARCH="amd64"
   GOHOSTOS="linux"
   GOOS="linux"
   GOPATH="/home/yewei/yyw/fabric/work"
   GORACE=""
   GOROOT="/home/yewei/go"
   GOTMPDIR=""
   GOTOOLDIR="/home/yewei/go/pkg/tool/linux_amd64"
   GCCGO="gccgo"
   CC="gcc"
   CXX="g++"
   CGO_ENABLED="1"
   CGO_CFLAGS="-g -O2"
   CGO_CPPFLAGS=""
   CGO_CXXFLAGS="-g -O2"
   CGO_FFLAGS="-g -O2"
   CGO_LDFLAGS="-g -O2"
   PKG_CONFIG="pkg-config"
   GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build424194477=/tmp/go-build -gno-record-gcc-switches"
   ```

3. 在`~/work/`创建一个`src/hello`文件夹，并创建文件`hello.go`文件，并敲入以下代码:

   ```go
   package main
   import "fmt"
   func main() {    
   	fmt.Printf("hello, world\n")
   }
   ```

4. 在~/work/下面执行`go install hello`，此时会构建`src/hello`里面的源码，生成`bin`文件夹