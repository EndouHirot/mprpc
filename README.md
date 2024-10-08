# mprpc
## 一. 介绍
本项目是由C++开发的分布式网络通信框架(RPC)，为了分布式环境下服务器之间进行远程方法调用提供了一套接口，通过该框架可以快速的实现RPC方法调用。

## 二. 技术栈
项目开发涉及到以下几个方面
- 集群和分布式概念以及原理
- RPC远程过程调用原理以及实现
- Protobuf数据序列化和反序列化
- ZooKeeper分布式一致性协调服务应用以及编程
- muduo网络库编程
- conf配置文件读取
- CMake构建项目集成编译环境
- github管理项目

## 三. 项目环境配置
由于项目使用了部分第三方库及构建环境，所以需要提前部署安装。
  ### 1.Protobuf安装配置
- protobuf（protocol buffffer）是google 的一种数据交换的格式，它独立于平台语言。google 提供了protobuf多种语言的实现：java、c#、c++、go 和 python，每一种实现都包含了相应语言的编译器以及库文件。由于它是一种二进制的  格式，比使用 xml（20倍） 、json（10倍）进行数据交换快许多。可以把它用于分布式应用之间的数据通信或者异构环境下的数据交换。作为一种效率和兼容性都很优秀的二进制数据传输格式，可以用于诸如网络传输、配置文件、数据存储等诸    多领域。

- ubuntu下 protobuf环境搭建  
    下载地址:https://github.com/google/protobuf
    
```
  1、解压压缩包：unzip protobuf-master.zip
  2、进入解压后的文件夹：cd protobuf-master
  3、安装所需工具：sudo apt-get install autoconf automake libtool curl make g++ unzip
  4、自动生成confifigure配置文件：./autogen.sh
  5、配置环境：./confifigure 
  6、编译源代码(时间比较长)：make
  7、安装：sudo make install
  8、刷新动态库：sudo ldconfig
```
  ### 2.Zookeeper安装配置

Zookeeper是在分布式环境中应用非常广泛，它的优秀功能很多，比如分布式环境中全局命名服务，服务注册中心，全局分布式锁等等，本项目只用到服务注册。参考链接：https://www.cnblogs.com/xinyonghu/p/11031729.html

下载地址:https://pan.baidu.com/s/1yPl1aNGuC7OVabs5icw1Ag?pwd=92xe 提取码: 92xe
```
1、下载zookeeper-3.4.10.tar.gz解压后进入zookeeper-3.4.10
2. cd zookeeper-3.4.10/conf
3. mv zoo_sample.cfg zoo.cfg
4. 进入bin目录，启动zkServer， ./zkServer.sh start
5. 可以通过netstat查看zkServer的端口，在bin目录启动zkClient.sh链接zkServer，熟悉zookeeper怎么组织节点
```
- **zk的原生开发API（c/c++接口）**

进入上面解压目录src/c下面，zookeeper已经提供了原生的C/C++和Java API开发接口，需要通过源码编译生成，过程如下：
```
~/package/zookeeper-3.4.10/src/c$ sudo ./confifigure
~/package/zookeeper-3.4.10/src/c$ sudo make
~/package/zookeeper-3.4.10/src/c$ sudo make install
```
主要关注zookeeper怎么管理节点，zk-c API怎么创建节点，获取节点，删除节点以及watcher机制的API编程。
- zk客户端常用命令
  
  ``ls、get、create、set、delete``
### 3.muduo网络库安装配置  
``【注意】：muduo库是基于boost开发的，所以需要先在Linux平台上安装boost库,安装方法参考链接：``[https://blog.csdn.net/QIANGWEIYUAN/article/details/88792874](https://blog.csdn.net/QIANGWEIYUAN/article/details/88792874)
  
muduo库源码github仓库地址：https://github.com/chenshuo/muduo  
```
  1、解压压缩包：unzip muduo-master.zip
  2、进入解压后的文件夹：cd muduo-master
  3、muduo库源码编译会编译很多unit_test测试用例代码，编译耗时长，我们也用不到，注释掉CMakeLists.txt中的  
    option(MUDUO_BUILD_EXAMPLES "Build Muduo examples" ON)
  4、安装cmake：sudo apt-get install cmake
  5、执行build.sh程序：./build.sh
  6、编译完成后，进行muduo库安装：./build.sh install
  7、把inlcude和lib目录下的文件拷贝到系统目录下：  
    cd build/release-install-cpp11/include/  
    mv muduo/ /usr/include/  
    cd build/release-install-cpp11/lib/  
    mv * /usr/local/lib/
  8、使用muduo库编写一个简单的服务器，测试muduo库是否可以正常使用
```

## 四. 项目目录结构
```
bin：可执行文件，这里放的是example编译好的二进制文件
build：项目编译文件
lib：项目库文件
src：源文件
test：放的是一些util和net的测试代码
example：框架代码使用范例
CMakeLists.txt：顶层的cmake文件
README.md：项目自述文件
autobuild.sh：一键编译脚本
```
- `autobuild.sh`是项目的一键编译脚本  
- 项目最终会编译完成后提供给用户的是一个个静态库和头文件目录，都放在lib下面  
  - `libmprpc.a`：rpc库
  - `include目录`

## 五. 项目分支
目前项目有三个分支：

  - `master`，该分支是基于muduo网络库作为通信基础的，所以在使用该分支代码的时候还需要安装muduo，安装方法参考链接: [https://blog.csdn.net/QIANGWEIYUAN/article/details/89023980](https://blog.csdn.net/QIANGWEIYUAN/article/details/89023980)  
  - `master_net`，该分支网络库是自己基于reactor模式开发的一套单线程c++网络库，部分代码参考muduo，可以供学习muduo。
  - `release/1.0`，基于master分支拉出来的一个稳定分支。

