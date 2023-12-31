# 包
包是函数和数据的集合，将有相关的函数和数据放在统一的文件/目录进行管理，每个包都可以作为独立的单元维护并提供给其他项目使用

## 声明
Go 源文件都需要在开头使用 package 声明所在包，包名告知编译器哪些源代码用于编译库文件，其次包名用于限制包内成员对外的可见性，最后包名用于在对公开成员的访问

包名使用简短的小写字母，常与目录名保持一致，一个包中可以有多个源文件，但必须使用相同包名
## 导入&调用
在使用包时需要使用 import 将包按路径导入，再通过包名调用成员，可通过 import 每行导入一个包，也可以使用括号包含所有包并使用一个 import 导入

工作目录说明
- bin：用于放置发布的二进制程序
- pkg：用于放置发布的库文件
- src：用于放置源代码


运行：
1. 将chapter08/gv 目录添加到 GOPATH 环境变量中
2. 编译&运行
	- 使用 go build 编译二进制文件
		
		命令：go build gpkgmain 
		
		说明：编译路径 gpkgmain 下的包，main 包，则在当前目录产生以 pkgmain 命名的二进制程序
	
	- 使用 go run 运行二进制文件
		
		命令：go run gpkgmain
	- 使用 go install 编译并发布二进制文件	
		
		命令：go install gpkgmain
		
		说明：编译并发布 gpkgmain 下的包，main 包，则在编译以后的以 pkgmain 命名的二进制程序拷贝到 bin 目录
	
	- 使用 go install 编译并发布库文件
		
		命令：go install gpkgname/pkg01
		
		说明：编译并发布路径 gpkgname/pkg01 下的包，非 main 包，则再将编译的以包名命名的库文件拷贝到 pkg/GOOS_GOARCH 目录下

	- 使用 go install 编译发布所有二进制和库文件
		
		命令：go install ./···

3. 包的循环导入
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202312312230771.png)

>[!ATTENTION]
>A 包导入 B 包，B 包导入 C 包，C 包导入 A 包，会形成循环导入，编译器会报错。

4. 导入形式
	1. 绝对路径导入

		在 GOPATH 目录中查找包

		示例：
		- `import "fmt"`
		- `import "gpkgname/pkg01"`

	2. 相对路径导入
	
		在当前文件所在目录查找
		
		示例：`import "./gpkgname/pkg02"`

	3. 点导入
	
		在调用点导入包中的成员时可以直接使用成员名称进行调用（省略包名）
		
		```go
		package main
		
		import "fmt"
		
		func main() {
			Println("hello world")
		}
		```
	
	4. 别名导入
	
	   到导入不同路径的相同报名时，可以别名导入为包重命名，避免冲突
	
	   ```go
	   import f "fmt"

	   func main() {
		   fmt.Println("Hello World")
	   }
	   ```
	
	5. 下划线导入
		
		Go 不允许包导入但未使用，在某些情况下需要初始化包，使用空白符作为别名进行导入，从而使得包中的初始化函数可以执行

		```go
		package main 

		func main() {
		}
		```

## Go 包管理
### 介绍
Go1.11 版本提供 Go modules 机制对包进行管理，同时保留 GOPATH 和 vender 机制，使用临时环境变量 GO111MODULE 进行控制，GO111MODULE 进行控制，GO111MODULE 有三个可选值：

1. 当 GO111MODULE 为 off 时，构建项目始终在 GOPATH 和 vender 目录搜索目标程序依赖包
2. 当 GO111MODULE 为 on 时，构建项目始终使用 Go module





