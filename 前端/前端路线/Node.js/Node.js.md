# Node.js

## Node.js 简介

Node.js是一个构建在V8引擎之上的JavaScript运行环境。它使得JS可以运行在浏览器以外的地方。相对于大部分的服务端语言来说，Node.js有很大的不同，它采用了单线程，且通过异步的方式来处理并发的问题。

- 运行在服务器端的JS
- 用来编写服务器
- 特点：
  - 单线程、异步、非阻塞
  - 统一 API

### Node.js 和 JavaScript 区别

JavaScript 组成

- ECMAScript（European Computer Manufacturer Association）
- DOM（node 没有）
- BOM（node 没有）

DOM、BOM 中有用的 API node.js 进行了保留，如 `setTimeout()、console.log`

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

## 异步编程

### 进程和线程

- 进程（厂房）
  - 程序的运行环境

- 线程（工人）
  - 线程是实际进行运算的东西

### 同步和异步

#### 同步

- 通常代码都是自上而下一行一行执行的
- 前面的代码不执行，后面的代码也不会执行
- 同步的代码执行会出现阻塞的情况

比如现实生活中

1. 点菜
2. 厨师做菜
3. 吃

#### 解决同步问题

- Java Python

  - 通过多线程来解决
- node.js
- 通过异步方式来解决

#### 异步 

- 一段代码的执行不会影响其他的程序

- 异步的问题

  - 异步的代码无法通过 return 来设置返回值

    ```js
    function sum(a, b) { 
      const begin = Date.now();
      
      setTimeout(() => {
        return a + b;
      }, 10000);  
    }
    
    console.log("1111111");
    
    const result = sum(123, 456);
    console.log('result', result);
    
    console.log('2222222');
    
    console.log('哈哈');
    console.log('嘻嘻');
    console.log('呵呵');
    ```

    ```
    1111111
    result undefined
    2222222
    哈哈
    嘻嘻
    呵呵
    ```

- 特点：

  1. 不会阻塞其他代码的执行

  2. 需要通过回调函数来返回结果 

     ```js
     function sum(a, b, callback) { 
       setTimeout(() => {
         callback(a + b);
       }, 10000);  
     }
     
     console.log("1111111");
     
     sum(123, 456, (result) => {
       console.log('result', result);
     });
     
     console.log('2222222');
     
     console.log('哈哈');
     console.log('嘻嘻');
     console.log('呵呵');
     ```

     ```
     1111111
     2222222
     哈哈
     嘻嘻
     呵呵
     result 579
     ```

- 基于回调函数的异步带来的问题

  1. 代码的可读性差

  2. 可调试性差

     ```js
     function sum(a, b, callback) { 
       setTimeout(() => {
         callback(a + b);
       }, 1000);  
     }
     
     console.log("1111111");
     
     // 回调地狱（死亡金字塔）
     sum(123, 456, (result) => {
       sum(result, 777, (result) => { 
         sum(result, 888, result => {
           sum(result, 999, result => {
             sum(result, 10, result => {
               console.log('result',result);
             })
           })
         })
       });
     });
     
     console.log('2222222');
     
     console.log('哈哈');
     console.log('嘻嘻');
     console.log('呵呵');
     ```

     #### 解决回调函数的问题

     - 需要一个东西，可以代替回调函数来给我们返回结果
     - Promise 横空出世
       - Promise 是一个可以用来存储数据的对象，Promise 存储数据的方式比较特殊，这种特殊方式使得 Promise 可以用来存储异步调用的数据。

## Promise

异步调用要通过回调函数来返回数据，当我们进行一些复杂的调用时，会出现「回调地狱」

问题：

异步必须通过回调函数来返回结果，回调函数一多就很痛苦

Promise

- Promise 可以帮助我们解决异步中的回调函数的问题
- Promise 就是一个用来存储数据的容器
  - 它拥有着一套特殊的存储数据的方式
  - 这个方式使得它里面可以存储异步调用的结果

### 创建Promise

Promise存储值的方式非常的特别，我们先来看看它的构造函数：

```js
const promise = new Promise(executor)
```

创建Promise时需要一个executor（执行器）为参数，执行器是一个回调函数，进一步调用它大概长这个样子：

```js
const promise = new Promise((resolve, reject) => {})
```

回调函数在执行时会收到两个参数，两个参数都是函数。第一个函数通常命名为resolve，第二个函数通常会命名为reject。向Promise中存储值的关键就在于这两个函数，可以将想要存储到Promise中的值作为函数的参数传递，像是这样：

```js
const promise = new Promise((resolve, reject) => {
    resolve("哈哈")
})
```

这样我们就将”哈哈”这个字符串存储到了Promise中，那么问题又来了，为什么需要两个函数存储值呢？很简单，resolve用来存储运行正确时的数据，reject用来存储运行出错时的错误信息。我们在使用Promise时需要根据不同的情况，调用不同的函数来存储不同的数据。

Promise为什么整了如此复杂的一种方式来存储数据呢？如果仅仅是存储其他的数据，这么做确实有点脱了放。但是Promise是专门为了异步调用而生的，所以Promise中存储的主要是异步调用的数据，也就是那些本来需要通过回调函数来传递的数据。在Promise中，可以直接调用异步代码，在异步代码执行完毕后直接调用resolve或reject来将执行结果存储到Promise中，这就解决了异步代码无法设置返回值的问题。换句话说，异步代码的执行结果可以直接存储到Promise中，像是这样：



```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("哈哈")
    }, 10000)
})
```

上例中通过setTimeout实现了一个异步调用，定时器会在10秒后执行，并调用resolve将”哈哈”存储到Promise中。现在你应该能够理解为什么Promise有这么一个奇怪的存储值的方式了吧？

### 获取Promise中的数据

Promise存储数据的方式奇特，读取方式同样特殊。我们需要通过Promise的实例方法来读取存储到Promise中的数据。现在我们有这样一个Promise，Promise中通过resolve存储了一个数据”哈哈”：



```js
const promise = new Promise((resolve, reject)=>{
    setTimeout(() => {
        resolve("哈哈")
    }, 10000)
})
```

#### Then

then是Promise的实例方法，通过该方法可以获取到Promise中存储的数据。它需要一个回调函数作为参数，Promise中存储的数据会作为回调函数的实参返回给我们：



```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("哈哈")
    }, 10000)
})

promise.then((data) => {
    console.log(data) // "哈哈"
})
```

注意：这种方式只适合读取通过resolve存储的数据，如果存储数据时出现了错误，或者是通过reject存储的数据，这种方式是读取不到的：



```js
const promise = new Promise((resolve, reject) => {
    throw new Error("出错了！")
    setTimeout(() => {
        resolve("哈哈")
    }, 10000)
})

promise.then((data) => {
    console.log(data)
})
```



```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject("哈哈")
    }, 10000)
})

promise.then((data) => {
    console.log(data)
})
```

上边的两块代码都是读取不到数据的，而且运行时会在控制台报出错误信息。这是因为，then的第一个参数只负责读取Promise中代码正常执行的结果，也就是只有Promise中数据正常时才会被调用。当Promise中的代码出错，或通过reject来添加数据时，我们还需要为其指定第二个参数来处理错误。

then的第二个参数同样是一个回调函数，两个回调的函数的结构相同，不同点在于第一个回调函数会在没有异常时被调用。而第二个函数会在出现错误（或通过reject存储数据）时调用。



```js
const promise = new Promise((resolve, reject) => {
    throw new Error("主动抛出错误")
    setTimeout(() => {
        resolve("哈哈")
    }, 10000)
})

promise.then((data) => {
    console.log(data)
}, (err) => {
    console.log("出错了", err)
})
```



```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject("哈哈")
    }, 10000)
})

promise.then((data) => {
    console.log(data)
}, (err) => {
    console.log("出错了", err)
})
```

上边两个案例中，then的第二个回调函数会执行。执行时异常信息或时reject中返回的数据会作为参数传递。现实开发中，第二个回调函数通常会用来编写异常处理的代码。

#### 原理

在Promise中维护着两个隐藏的值PromiseResult和PromiseState，PromiseResult是Promise中真正存储值的地方，在Promise中无论是通过resolve、reject还是报错时的异常信息都会存储到PromiseResult中。PromiseState用来表示Promise中值的状态，Promise一共有三种状态：pending、fulfilled、rejected。pending是Promise的初始化状态，此时Promise中没有任何值。fulfilled是Promise的完成状态，此时表示值已经正常存储到了Promise中（通过resolve）。rejected表示拒绝，此时表示值是通过reject存储的或是执行时出现了错误。

当我们调用Promise的then方法时，相当于为Promise设置了一个回调函数，换句话说，then中的回调函数不会立即执行，而是在Promise的PromiseState发生变化时才会执行。如果PromiseState从pending变成了fulfilled则then的第一个回调函数执行，且PromiseResult的值作为参数传递给回调函数。如果PromiseState从pending变成了rejected则then的第二个回调函数执行，且PromiseResult的值作为参数传递给回调函数。

![img](https://my-wp.oss-cn-beijing.aliyuncs.com/wp-content/uploads/2022/10/20221010132511667-1024x574.png)

then执行后每次总会返回一个新的Promise，并将then中回调函数的返回值存储到这个Promise中，如果没有指定返回值则新Promise中不会存储任何值。



```js
const promise = new Promise((resolve, reject)=>{
    resolve("第一步执行结果")
})

const promise2 = promise.then(result => {
    console.log("收到结果：", result)
    return "第二步执行结果" // 会作为新的结果存储到新Promise中
})

const promise3 = promise2.then(result => {
    console.log("收到结果：", result)
    return "第三步执行结果" // 会作为新的结果存储到新Promise中
})
```

在简化一些可以这样写：



```js
const promise = new Promise((resolve, reject)=>{
    resolve("第一步执行结果")
})

promise.then(result => {
    console.log("收到结果：", result)
    return "第二步执行结果" // 会作为新的结果存储到新Promise中
}).then(result => {
    console.log("收到结果：", result)
    return "第三步执行结果" // 会作为新的结果存储到新Promise中
})
```

第一个then用来读取上边我们创建的Promise中存储的结果，第二个then用来读取第一个then所返回的结果，依此类推我们就可以根据需要一直then下去，如此便解决了“回调地狱”的问题。

有了Promise后，在异步函数中我们便不再需要通过回调函数来返回结果，取而代之的是返回一个Promise，并将异步执行的结果存储到Promise中，像是这样：



```js
function sum(a, b){
    return new Promise((resolve, reject) => {
        setTimeout(()=>{
            resolve(a + b)
        }, 10000)
    })
}
```

由于sum的返回值是一个Promise，所以我们不在需要通过回调函数来读取结果：



```js
sum(123, 456).then(result => {
    console.log("结果为：", result) // 结果为： 579
})
```

如果需要连续多次调用，也不会在有“回调地狱的问题”：



```js
sum(123, 456)
    .then(result => sum(result, 777))
    .then(result => sum(result, 888))
    .then(result => console.log(result))
```

#### Catch

除了then以外，Promise中还有一个catch方法，catch和then使用方式类似，但是catch中只需要一个回调函数作为参数。catch中回调函数的作用等同于then中的第二个回调函数，会在执行出错时被调用。既然有了then的第二个参数，为什么还需要一个catch呢？两个回调函数都写到then中，会导致代码不够清晰，但是多了一个catch后立刻就变的不一样了，开发时通常会在then中编写正常运行时的代码，catch中编写出现异常后要执行的代码：



```js
const promise = new Promise((resolve, reject) => {
    reject("出错了")
})


// 出现异常，then中只传了一个回调函数，无法读取数据
// promise.then((data) => {
//     console.log(data)
// }) 

// 出现异常，可以通过catch来读取数据
promise.catch(err => {
    console.log(err)
})
```

当Promise中代码执行出错时（或者reject执行时），如果我们调用的是catch来处理数据，则Promise会将错误信息传递给catch的回调函数，我们便可以在catch中处理异常，同时catch回调函数的返回值会作为下一步Promise中的数据向下传递。如果我们调用了then来处理数据，同时没有传递第二个参数，这时then是不会执行的，而是将错误信息直接添加到下一步返回的Promise中，由后续的方法处理。在后续调用中如果有catch或then的第二个参数，则正常处理。如果没有，则报错。

简言之，处理Promise时，如果没有对Promise中的异常进行处理（无论是then的二参数，还是catch），则异常信息总是会封装到下一步的Promise中进行传递，直到找到异常处理的代码位置，如果一直没有处理，则报错。

这种设计方式使得我们可以在任意的位置对Promise的异常进行处理，例如有如下代码：



```js
function sum(a, b) {
    return new Promise((resolve, reject) => {
        if (Math.random() > 0.7) {
            throw new Error("出错了")
        }
        resolve(a + b)
    })
}

sum(123, 456)
    .then(result => sum(result, 777))
    .then(result => sum(result, 888))
    .then(result => console.log(result))
```

上例代码中，sum函数有一定的几率会出现异常，但是我们并不确定何时会出现异常，这时有了catch就变的非常的方便，因为在出现异常后所有的then在异常处理前都不会执行，所以我们可以将catch写在调用链的最后，这样无论哪一步出现异常，我们都可以在最后统一处理。像是这样：



```js
sum(123, 456)
    .then(result => sum(result, 777))
    .then(result => sum(result, 888))
    .then(result => console.log(result))
    .catch(err => console.log("哎呀出错了，随便返回一个吧", 8888))
```

当然如果我们想在中间处理异常也是没有问题的，只是需要注意在链式调用中间处理异常时，由于后续还有then要执行，所以一定不要忘了考虑是否需要在catch中返回一个结果供后续的Promise使用：



```js
sum(123, 456)
    .then(result => sum(result, 777))
    .catch(err => {
        // 也可以在调用链的中间处理异常
        console.log("出错了，我选择忽略这个错误，重新计算")
        return sum(123, 456)
    })
    .then(result => sum(result, 888))
    .then(result => console.log(result))
    .catch(err => console.log("哎呀出错了，随便返回一个吧", 8888))
```

还有一点要强调一下，在Promise正常执行的情况下如果遇到catch，catch是不会执行的，此时Promise中的结果会自动传递给下一个Promise供后续使用。

#### Finally

finally也是Promise的实例方法之一，和then、catch不同，无论何种情况finally中的回调函数总会执行，通常我们在finally中定义一些无论Promise正确执行与否都需要处理的工作。注意，finally的回调函数不会接收任何参数，同时finally的返回值也不会成为下一步的Promise中的结果。简单说，finally只是编写一些必须要执行的代码，不会对Promise产生任何实质的影响。

## 静态方法

Promise的静态方法直接通过Promise类去调用，这些方法可以帮助我们完成一些更加复杂的异步操作。

### Promise.All

当我们有多个Promise需要执行，且需要多个Promise都执行完毕，在将他们的结果进行统一处理时，我们便可以使用Promise.all来帮助我们完成这项工作。

Promise.all(iterable)

all需要一个数组（可迭代对象）作为参数，数组中可以存放多个Promise。调用后，all方法会返回一个新Promise，这个Promise会在数组中所有的Promise都执行后完成，并返回所有Promise的结果。比如：



```js
function sum(a, b) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(a + b)
        }, 5000);
    })
}

Promise.all([sum(1, 1), sum(2, 2), sum(3, 3)])
    .then((result) => {
        console.log(result)
    })
```

上例中，调用三次sum，且将其添加到数组中传递给all。调用后all会返回一个新的Promise，当三次计算都完成后，新的Promise也会变为完成状态，并将三次执行的结果封装到数组中返回。

### Promise.AllSettled

all仅有当全部Promise都完成时才会返回有效数据，而allSettled用法和all一致，但是它里边无论Promise是否完成都会返回数据，只是他会根据不同的状态返回不同的数据。

成功：{status:”fulfilled”, value:result}

失败：{status:”rejected”, reason:error}

### Promise.Race

race会返回首先执行完的Promise，而忽略其他未执行完的Promise

### Promise.Any

any会race类似，但是它只会返回第一个成功的Promise，如果所有的Promise都失败才会返回一个错误信息。

### Promise.Resolve

Promise.resolve用来创建一个新的Promise实例，且直接通过resolve存入一个数据。

### Promise.Reject

Promise.reject用来创建一个新的Promise实例，且直接通过reject存入一个数据。

> [!note]
>
