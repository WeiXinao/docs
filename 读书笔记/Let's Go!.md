## 2.3 Web Application Basics
1. [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)：参考 [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)，这里不是很明白，大概指的是 Linux 下的网络接口配置。
2. [writes them to the standard error stream(which should display in your terminal window)](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=e07a1935-2574-7509-3f36-e515d9ab3b17&page=24)：golang log 包默认是把错误输出到标准错误（stderr），输出到标准错误输出流的错误也会显示在终端，与输出到标准输出（stdout）不同的是：

	- stderr 的实时性更强，stderr 是没有缓冲的，发生错误后会马上输出。
	- 当 stdout 阻塞时发生错误，错误不会阻塞，会马上显示到终端。








