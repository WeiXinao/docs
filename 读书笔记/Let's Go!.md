## 2.3 Web Application Basics
### Additional Information
#### Network Address
1. [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)：参考 [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)，这里不是很明白，大概指的是 Linux 下的网络接口配置。
2. [writes them to the standard error stream(which should display in your terminal window)](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=e07a1935-2574-7509-3f36-e515d9ab3b17&page=24)：golang log 包默认是把错误输出到标准错误（stderr），输出到标准错误输出流的错误也会显示在终端，与输出到标准输出（stdout）不同的是：

	- stderr 的实时性更强，stderr 是没有缓冲的，发生错误后会马上输出。
	- 当 stdout 阻塞时发生错误，错误不会阻塞，会马上显示到终端。

### Fixed Path and Subtree Patterns
go 的 servemux 支持两种不同类型的 URL 模式，固定路径和子树路径，固定路径不以斜杠结尾，而子树路径以斜杠结尾。

我们的两种模式 —— "/snippet" 和 "/snippet/create"







