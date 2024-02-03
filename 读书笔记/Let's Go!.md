## 2.3 Web Application Basics
### Additional Information
#### Network Address
1. [network interfaces](obsidian://bookmaster?type=open-book&bid=IzMXnQxEBWDTXjmG&aid=62f8412e-3721-3ffe-4868-0e1bfcd07ae7&page=24)：参考 [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)，这里不是很明白，大概指的是 Linux 下的网络接口配置。
2. [writes them to the standard error stream(which should display in your terminal window)](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=e07a1935-2574-7509-3f36-e515d9ab3b17&page=24)：golang log 包默认是把错误输出到标准错误（stderr），输出到标准错误输出流的错误也会显示在终端，与输出到标准输出（stdout）不同的是：

	- stderr 的实时性更强，stderr 是没有缓冲的，发生错误后会马上输出。
	- 当 stdout 阻塞时发生错误，错误不会阻塞，会马上显示到终端。

### [Fixed Path and Subtree Patterns](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=6966db16-e081-2980-e552-48f82e55f4ab&page=28)
Go 的 servemux 支持两种不同类型的 URL 模式，固定路径和子树路径，固定路径不以斜杠结尾，而子树路径以斜杠结尾。

我们的两种模式 —— `"/snippet"` 和 `"/snippet/create"` 都是 固定路径的例子，在 Go 的 servemux, 像这些固定路径模式只有当请求的 URL 路径准确匹配时，才会被匹配（并且调用相应的处理器）；

相反，我们的 `"/"` 模式是一个 subtree path 的例子（因为它以"/"结尾），其他例子比如 `"/static/"`。子树路径模式（subtree path patterns），当请求 URL 路径的开头被子树路径（subtree path）匹配时，就会匹配（并且相应的处理器被调用），你可以认为子树路径（subtree path）的行为有点像它们在结尾有通配符，例如：`"/**"` 或者 `"/static/**"`

这就帮助解释了为什么 `"/"` path 会匹配所有。这个模式本质上意味着匹配后面跟随着任何字符串的单斜杠（或者什么都不匹配）。







