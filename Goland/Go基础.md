# Go 基础

## goland 环境安装

### Windows 平台

#### 配置环境变量

复制框中路径

![image-20230302171501067](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302171501067.png)

右击**此电脑**，选择**属性**

![image-20230302173306927](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302173306927.png)

下滑，选择**系统信息**

![image-20230302173715058](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302173715058.png)

相关链接一栏，选择**高级系统设置**

![image-20230302173844039](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302173844039.png)

选择**环境变量**

![image-20230302174207645](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302174207645.png)

在系统变量一栏中，选择 **Path**

![image-20230302174333761](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302174333761.png)

将**复制的路径**粘贴到**环境变量**中

![image-20230302174545689](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302174545689.png)

在 **terminal** 中 输入 `go version`，如果出现**版本号**，则安装成功

![image-20230302174719779](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302174719779.png)

#### 配置 go 环境

打开 powershell，输入如下命令：

```go
$env:GO111MODULE = "on"
$env:GOPROXY = "http://goproxy.cn"
```

输入 `go env`， 查看是否修改成功

![image-20230302180026496](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302180026496.png)

GOROOT：代表 go 的安装路径

GOPATH：代表 go 的项目安装路径，之前必须通过 GOPATH 创建项目，现在有了模块化管理，基本上也不用了，了解即可。

> [!attention]
>
>  使用 go mod 管理库，需要科学上网

#### 安装配置 git

```
https://git-scm.com/download
```

安装过程一路 next

配置环境变量，命令行输入 git 测试

![image-20230302181233424](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302181233424.png)

出现**选项提示**，表示安装成功

#### GOROOT 和 GOPATH

GOROOT 就是 go 安装的根目录，GOPATH 就是 go 项目所在的路径，高版本 go 项目已经不再依赖 gopath 来管理项目，使用 go mod 来管理项目。

> 参考资料：
>
> - [(111条消息) 【Go报错】go go.mod file not found in current directory or any parent directory 错误解决_go: go.mod file not found in current directory or _普通网友的博客-CSDN博客](https://blog.csdn.net/m0_67401417/article/details/126080567)

### Linux 平台

:point_right: [Linux -> CentOS 安装 go 环境 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1847079)

## 开发工具简介

golang 的开发工具有很多，例如：

1. vim 
2. sublime
3. atom
4. LitelDE
5. eclipse
6. **goland**
7. **vscode**

### 使用 goland 开发 go 应用

goland 是一款付费、功能强大的 goland 集成开发环境，是 Jetbrain 公司的产品。下载地址：`https://www.jetbrains.com/go/`，goland 非常智能，几乎不用配置，安装即用。

### 使用 vscode 开发 go 应用

下载安装 vscode `https://code.visualstudio.com/`

```
go mod init helloGo
```

helloGo：表示 module 名字，随自己喜欢

## 查看可用命令

直接在终端中输入 `go help` 即可显示所有的 go 命令以及相应命令功能简介，主要有下面这些：

- build：编译包和依赖

  ![image-20230302190530516](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302190530516.png)

- clean：移除对象文件

  ![image-20230302221425519](C:\Users\小新\AppData\Roaming\Typora\typora-user-images\image-20230302221425519.png)

- doc：显示包或者符号的文档

- env：打印 go 的环境信息

  ![image-20230302221638146](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302221638146.png)

- bug：启动错误报告

- fix：运行 go tool fix

- fmt：运行 go fmt 进行格式化

  ![](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302222226886.png)

  ![](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/image-20230302222536495.png)

- generate：从 processing source 生成 go 文件

- get：下载并安装包和依赖

- install：编译并安装包和依赖

- list：列出包

- run：编译并运行 go 程序

- test：运行测试

- tool：运行 go 提供的工具

- version：显示 go 的版本

- vet：运行go tool vet
