## 2.3 Web Application Basics
### Additional Information
#### Network Address
1. [network interfaces](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=acb76dea-1aa7-e584-5ac8-012e88944e23&page=24)：参考 [network interfaces](obsidian://bookmaster?type=open-book&bid=FTcyhwpUReOpCYuo&aid=9a979fe8-11fc-7efb-ca20-bc39151b31e7&page=24)，这里不是很明白，大概指的是 Linux 下的网络接口配置。
2. [writes them to the standard error stream(which should display in your terminal window)](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=e82e60b9-2c5e-d274-3247-c88dfdf1cf92&page=24)：golang log 包默认是把错误输出到标准错误（stderr），输出到标准错误输出流的错误也会显示在终端，与输出到标准输出（stdout）不同的是：

	- stderr 的实时性更强，stderr 是没有缓冲的，发生错误后会马上输出。
	- 当 stdout 阻塞时发生错误，错误不会阻塞，会马上显示到终端。

### [Fixed Path and Subtree Patterns](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=33cf4a32-39ca-5844-0718-37b1f4c32dc9&page=28)

Go 的 servemux 支持两种不同类型的 URL 模式，固定路径和子树路径，固定路径不以斜杠结尾，而子树路径以斜杠结尾。

我们的两种模式 —— `"/snippet"` 和 `"/snippet/create"` 都是 固定路径的例子，在 Go 的 servemux, 像这些固定路径模式只有当请求的 URL 路径准确匹配时，才会被匹配（并且调用相应的处理器）；

相反，我们的 `"/"` 模式是一个 subtree path 的例子（因为它以"/"结尾），其他例子比如 `"/static/"`。子树路径模式（subtree path patterns），当请求 URL 路径的开头被子树路径（subtree path）匹配时，就会匹配（并且相应的处理器被调用），你可以认为子树路径（subtree path）的行为有点像它们在结尾有通配符，例如：`"/**"` 或者 `"/static/**"`

这就帮助解释了为什么 `"/"` path 会匹配所有。这个模式本质上意味着匹配后面跟随着任何字符串的单斜杠（或者什么都不匹配）。

## 2.5 Customizing HTTP Headers
1. [non-idempotent](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=323dddb5-2966-b8eb-bd68-fc2b3e49a0e2&page=35)：幂等性

### HTTP Status Code
| 状态码 | 含义 |
| ---- | ---- |
| [405](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=3cf1b47a-c5ac-d5f1-4b52-d0e7980b48b7&page=36) | method not allowed |
| [301](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=05c92eb3-aa02-aa9b-3dfe-4259415ffb69&page=33) | Permanent Redirect |

### Addtitional Information 
#### [Manipulating the Header Map](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=653862a4-5f3f-4a8c-6b75-641bafc1a8fd&page=41)
```go
// Set a new cache-control header. If an existing "Cache-Control" header exists
// it will be overwritten.
w.Header().Set("Cache-Control", "public, max-age=31536000")

// In contrast, the Add() method appends a new "Cache-Control" header and can
// be called multiple times.
w.Header().Add("Cache-Control", "public")
w.Header().Add("Cache-Control", "max-age=31536000")

// Delete all values for the "Cache-Control" header.
w.Header().Del("Cache-Control")

// Retrieve the first value for the "Cache-Control" header.
w.Header().Get("Cache-Control")
```

#### Head Canonicalization

>[!NOTE]
>[Note: When headers are written to a HTTP/2 connection the header names and values will always be converted to lowercase, as per the specifications.](obsidian://bookmaster?type=open-book&bid=gNZeRcxcHTYvWxQm&aid=373f603b-8b9e-9c29-c38e-d286681e23dd&page=42)
>
>当头被写到 HTTP/2 连接中时，头的名称和值总是会被转化成小写。












