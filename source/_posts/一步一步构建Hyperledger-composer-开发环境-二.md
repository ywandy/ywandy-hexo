---
title: 一步一步构建Hyperledger composer 开发环境(二)
date: 2018-08-15 14:53:43
tags:
    - 区块链
    - hyperledger
    - composer
    - nodejs
categories: 
    - 编程笔记
---

## 安装Hyperledger composer  
### 一、安装前准备:  
- 操作系统: ubuntu 14.04/16.04 LTS  
- Docker: 17.03或者更高 
- Docker-Compose: 1.8或者更高 
- Node: 8.9 或者更高(大于9.0版本不兼容)
- npm: v5.x
- git: 2.9.x 或者更高
- 代码编辑器，例如Visual Studio Code  

### 二、安装需要的组件:  
使用以下的命令从网上下载上述的依赖:  

``` bash
curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh
chmod u+x prereqs-ubuntu.sh
./prereqs-ubuntu.sh
```

以上步骤完成后，就可以开始下面的步骤，安装开发环境.  

### 三、安装开发环境:  
#### （1）安装Cli工具:  
**Composer**自带了很多的命令行工具。最重要的是`composer-cli`，它包含所有必要的操作，接下来安装的`generator-hyperledger-composer`，`composer-rest-server`以及`Yeoman`，在构建于业务网络交互的应用程序会很有用。  
1. 工具安装:  
``` bash
npm install -g composer-cli@0.20
```
2. REST Server 安装：
``` bash
npm install -g composer-rest-server@0.20
```
3. 生成器的安装：
``` bash 
npm install -g generator-hyperledger-composer@0.20
```
4. Yeoman安装(一个生成应用程序的工具)：
``` bash
npm install -g yo
```

#### （2）安装Playground:

1. 安装composer-playground：

   ``` bash
   npm install -g composer-playground@0.20
   ```

#### （3）配置IDE开发环境:

**我们使用visual studio code去做开发的IDE：**
1. 从以下URL安装VSCode：[https](https://code.visualstudio.com/download)：//code.visualstudio.com/download
2.  打开VSCode，转到Extensions，然后`Hyperledger Composer`从Marketplace中搜索并安装扩展程序。

#### （4）安装Hyperledger Fabric：

**这个安装步骤提供部署业务网络的本地Hyperledger Fabric运行时** ：

1. 选择一个目录，例如`~/fabric-dev-servers `，执行以下代码:

   ``` bash
   mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers
   curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
   tar -xvf fabric-dev-servers.tar.gz进入进入刚刚下载的那个目录:
   ```

2. 进入刚刚下载的路径: 

   ``` bash
   cd ~/fabric-dev-servers
   export FABRIC_VERSION=hlfv12
   ./downloadFabric.sh
   ```

   

### 四、控制你的开发环境:  

#### （1）启动和停止Hyperledger Fabric  

``` bash
cd ~/fabric-dev-servers
export FABRIC_VERSION=hlfv12
./startFabric.sh
./createPeerAdminCard.sh
```

停止运行时：`~/fabric-dev-servers/stopFabric.sh`

启动运行时：`~/fabric-dev-servers/startFabric.sh`

（当运行了停止脚本，下次运行除了执行`startFabric.sh`，还需要运行`createPeerAdminCard.sh`

#### （2）启动网络应用程序（playground）

``` bash
composer-playground
```

然后使用浏览器访问 [http://localhost:8080/login](http://localhost:8080/login)  就可以打开**playground**



### 五、干净的销毁网络:

1. 执行`stopFabric.sh` 先关闭所有的容器:

   ``` bash
   cd ~/fabric-dev-servers
   ./stopFabric.sh
   ```

2. 执行`docker container prune` 去消除所有的**fabric** 容器

3. 执行`sudo rm ~/.composer/* -rf`清除**composer**网络残留的一些chaindata文件
