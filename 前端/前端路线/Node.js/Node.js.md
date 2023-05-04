# Node.js

## Node.js 简介

Node.js是一个构建在V8引擎之上的JavaScript运行环境。它使得JS可以运行在浏览器以外的地方。相对于大部分的服务端语言来说，Node.js有很大的不同，它采用了单线程，且通过异步的方式来处理并发的问题。

- 运行在服务器端的JS
- 用来编写服务器
- 特点：
  - 单线程、异步、非阻塞
  - 统一 API

### node.js 和 JavaScript 区别

JavaScript 组成

- ECMAScript（European Computer Manufacturer Association）
- DOM（node 有）
- BOM（node 没有）

DOM 

## 安装

### 直接安装

可以通过多种不同的方式在计算机中安装Node.js，其中最简单的方式便是直接在官网下载安装，官网地址：https://nodejs.org/en/，在官网点击对应的版本即可自动下载Node.js。

下载后，双击安装包根据提示一步一步安装即可，安装时建议不要修改安装路径，直接安装到默认位置。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/09/20220927082054994.png)

安装出现这个界面时，勾选上面的选项，主要是安装一些必要的工具（像python）如果不勾选将会有一些的模块将无法使用（也可以安装完node后再单独安装这些）。最后一步一步按照提示点击安装，等待进度条读完即可。

安装完毕打开命令行窗口，输入`node -v`，出现node版本号，即表示安装成功。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/09/20220927082624151.png)

### 使用安装工具Nvm

除了直接安装外，也可以通过安装工具来安装，使用安装工具安装后更方便我们在不同的node版本之间进行切换，使用起来更加灵活。

这里我们以window下的nvm为例来演示，首先打开https://github.com/coreybutler/nvm-windows/releases，下载最新版的nvm-setup.exe。根据提示下一步下一步即可，同样推荐安装到默认路径。在命令行中输入`nvm version`后，出现版本即表示安装成功。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/09/20220927085852254.png)

需要注意的是，此处仅仅是安装了nvm，并没有安装node，接下来我们还需要通过命令行的形式安装node。输入`nvm install latest`下载并安装最新版的node，输入`nvm install lts`安装稳定版的node，也可以输入版本号，安装指定版本node。下载需要花费一定的时间，请耐心等待。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/09/20220927090337962.png)

输入`nvm use latest`切换到最新版node，输入`nvm use lts`切换到稳定版node，也可以输入版本号来切换到指定版本。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/09/20220927093949514.png)

### NVM配置镜像服务器

nvm毕竟还是国外的软件，由于一些特殊原因在国内访问时会有无法下载node的情况出现，这时我们只需要将nvm的服务器修改为国内的镜像服务器即可解决该问题。比如，可以通过如下代码将nvm的node镜像服务器修改为国内的阿里云：

```bash
nvm node_mirror https://npmmirror.com/mirrors/node/
```

### NVM 常用命令

```bash
nvm list                                 显示已安装的 node 版本
nvm install 版本						    安装指定版本的 node
nvm use 版本								指定要使用的 node 版本
```

