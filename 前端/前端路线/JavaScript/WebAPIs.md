# WebAPIs

## DOM 树

- DOM 树是什么
  - 将 HTML 文档以树状结构直观的表现出来，我们称之为文档树或 DOM 树
  - 描述网页内容关系的名词
  - 作用：<font color=#F36208>文档树直观的体现了标签与标签之间的关系</font>

## DOM 对象⭐

- DOM 对象：浏览器根据 html 标签生成的 <font color=#F36208>JS对象</font> 
  - 所有的标签属性都可以在这个对象上面找到
  - 修改这个对象的属性会自动映射到标签身上
- DOM 的核心思想
  - 把网页内容作为<font color=#F36208>对象</font>来处理
- document 对象
  - 是 DOM 里提供的一个<font color=#F36208>对象</font>
  - 所以它提供的属性和方法都是<font color=#F36208>用来访问和操作网页内容的</font>
    - 例：`document.write()`
  - 网页所有内容都在 document 里面

## 总结

1. DOM 树是什么？
   -  将 HTML 文档以树状结构直观的表现出来，我们称之为文档树或 DOM 对象
   -  作用：<font color=#F36208>文档树直观的体现了标签与标签之间的关系</font>
2. DOM 对象是怎么创建的？
   - 浏览器根据 HTML 标签生成的 <font color=#F36208>JS对象（BOM对象）</font>
   - DOM 的核心就是把内容当<font color=#F36208>对象</font>来处理
3. document 是什么？
   - 是 DOM 里提供的一个<font color=#F36208 style="font-weight: 700">对象</font>
   - 网页所有的内容都在 document 里面

## 获取 DOM 对象

```markdown
目标：能查找/获取 DOM 对象
提问：我们想要操作某个标签首先做什么？
	- 首先选中这个标签，跟 CSS 选择器类似，选中标签才能操作 
- 查找 DOM 元素就是利用 JS 选择页面中标签元素
学习路径：
1. 根据 CSS 选择器来获取 DOM 元素（重点）
2. 其他获取 DOM 元素的方法（了解） 
```

### 根据 CSS 选择器来获取 DOM 元素 ⭐

1. 选择匹配第一个元素

**语法：**

```javascript
document.querySelector('css选择器')
```

**参数：**

包含一个或多个有效的 CSS 选择器 <font color=#F36208 style="font-weight : bold">字符串</font>

**返回值：**

CSS 选择器匹配的第一个元素，一个 HTMLElement 对象。
如果没有匹配到，则返回null。

👉 多参看文档： https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector

2. 选择匹配的多个元素

**语法：**

```javascript
document.querySelectorAll('css选择器')
```

得到的是一个<font color=#F36208 style="font-weight: bold">伪数组</font>：

- 有长度有索引号的数组
- 但是没有 `pop() push()` 等数组方法
  想要得到里面的每一个对象，但需要遍历（for）的方式获得。

> [!note]
>
> 注意事项：
>
> 哪怕只有一个元素，通过 `querySelectAll()` 获取过来的也是一个<font color=#F36208 style="font-weight: bold">伪数组</font>，
> 里面只有一个元素而已

**参数：**

包含一个或多个有效的 CSS选择器 <font color=#F36208 style="font-weight: bold">字符串</font>

**返回值：**

CSS 选择器匹配的 <font color=#F36208>NodeList 对象集合</font>

**例如：**

```javascript
document.querySelectorAll('ul li')
```

### 总结

1. 获取一个 DOM 元素我们使用谁？能直接操作修改吗？
   - `querySelector()`
   - 可以
2. 获取多个 DOM 元素我们使用谁？能直接操作修改吗？ 如果不能可以怎么做到修改？
   - `querySelectorAll()`
   - 不可以，只能通过遍历的方式一次给里面的元素做修改

### 练习

请控制台依次输出 3 个 li 的 DOM 对象

![](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/code.png)

**参考代码：**

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>输出DOM对象</title>
</head>
<body>
    <ul class="nav">
        <li>我的首页</li>
        <li>产品介绍</li>
        <li>联系方式</li>
    </ul>
    <script>
        // 1. 获取元素
        const lis = document.querySelectorAll('.nav li');
        console.log(lis);
        lis.forEach(element => {
           console.log(element); // 每个 li 对象  
        });

        // const p = document.querySelectorAll('.nav');
        // p[0].style.color = 'red';
    </script>
</body>
</html>
```

👉[演示demo](#)

### 总结

1. 获取页面中的标签我们最终常用哪两种方式？
   - `querySelectorAll()`
   - `querySelector()`
2. 它们两者的区别是什么？
   - `querySelector()` 只能选择一个元素，可以直接操作
   - `querySelectorAll()` 可以选择多个元素，得到的是伪数组，需要遍历得到每一个元素
3. 他们两者小括号里面的参数有什么注意事项？
   - 里面写 CSS 选择器 
   - <font color=#F36208 style="font-weight: bold;">必须是字符串，也就是必须加引号</font>

# 操作元素内容

- 对象.innerText 属性
- 对象.innerHTML 属性

## 1. 元素.innerText 属性

- 将文本内容添加/更新到任意标签位置
- 显示纯文本，不解析标签

> [!note]
>
>
> 举例说明：
>
> ```java
> // 1. 获取元素
> const box = document.querySelector('.box');
> // 2. 修改文字内容 对象.innerText 属性
> console.log(box.innerText); // 获取文字内容
> box.innerText = '<strong>我是一个盒子</strong>';  // 修改文字内容
> ```


## 2. 元素.innerHTML 属性

- 将文本内容添加/更新到任意标签位置
- 会解析标签，多标签建议使用模板字符

> [!note]
>
>
> 举例说明：
>
> ```javascript
> // 3. innerHTML 解析标签
> console.log(box.innerHTML);
> box.innerHTML = '我要更换';
> box.innerHTML = '<strong>我要更换</strong>'
> ```

## 总结

1. 设置/修改 DOM 元素内容有哪两种方式？
   - 元素.innerText 属性
   - 元素.innerHTML 属性
1. 两者的区别是什么？
   - 元素.innerText 属性，只识别文本，不能解析标签
   - <font color=#F36208>元素.innerHTML 属性，能识别文本，能够解析标签</font>
   - <font color=#F36208>如果你还在纠结到底用谁，你可以选择 innerHTML</font>

## 案例：年会抽奖案例

需求：从数组随机抽取一等奖、二等奖和三等奖，显示到对应的标签里面。

分析：

1. 声明数组：`const personArr = ['周杰伦', '刘德华', '周星驰', '张学友']`
2. 一等奖：随机生成一个数字（0~数组长度），找到对应数组的名字
3. 通过 `innerText` 或者 `innerHTML` 将名字写入 `span` 元素内部
4. 二等奖依次类推

![](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/202302230152381.png)

参考代码：

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>年会抽奖</title>
  <style>
    .wrapper {
      width: 840px;
      height: 420px;
      background: url(./images/bg01.jpg) no-repeat center / cover;
      padding: 100px 250px;
      box-sizing: border-box;
    }
  </style>
</head>

<body>
  <div class="wrapper">
    <strong>传智教育年会抽奖</strong>
    <h1>一等奖：<span id="one">???</span></h1>
    <h3>二等奖：<span id="two">???</span></h3>
    <h5>三等奖：<span id="three">???</span></h5>
  </div>
  <script>
    // 1. 声明数组
    const personArr = ['周杰伦', '刘德华', '周星驰', '张学友'];
    const prizes = document.querySelectorAll('div.wrapper span');
    // 2. 先做一等奖
    for (let i = 0; i < 3; i++) {
      const randomIndex = Math.floor(Math.random() * personArr.length);
      const selectElement = personArr[randomIndex];
      personArr.splice(randomIndex, 1);
      prizes[i].innerText = selectElement;
    }
  </script>
</body>

</html>
```

👉 [演示demo](#)

# 操作元素属性

- <font color=#F36208>操作元素<strong>常用</strong>属性</font>
- 操作元素<font color=#F36208 style="font-weight: bold">样式</font>属性
- 操作<font color=#F36208 style="font-weight: bold;">表单元素</font>属性
- 自定义属性



## 练习：页面刷新，页面随机更换背景图片

需求：当我们刷新页面，页面中的背景图片随机显示不同的图片

分析：

1. 随机函数
2. CSS页面背景图片 `background-image`
3. 标签选择 body，因为 body 是唯一的标签，可以直接写 `document.body.style`



1. 使用 className 和 classList 的区别？
   - `className` 和 `classList` 修改大量样式的更方便
   - `对象.style.属性` 修改样式不多时更方便
   - `classList` 是追加和删除不影响之前类名



随机轮播图

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>轮播图点击切换</title>
  <style>
    * {
      box-sizing: border-box;
    }

    .slider {
      width: 560px;
      height: 400px;
      overflow: hidden;
    }

    .slider-wrapper {
      width: 100%;
      height: 320px;
    }

    .slider-wrapper img {
      width: 100%;
      height: 100%;
      display: block;
    }

    .slider-footer {
      height: 80px;
      background-color: rgb(100, 67, 68);
      padding: 12px 12px 0 12px;
      position: relative;
    }

    .slider-footer .toggle {
      position: absolute;
      right: 0;
      top: 12px;
      display: flex;
    }

    .slider-footer .toggle button {
      margin-right: 12px;
      width: 28px;
      height: 28px;
      appearance: none;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 4px;
      cursor: pointer;
    }

    .slider-footer .toggle button:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .slider-footer p {
      margin: 0;
      color: #fff;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .slider-indicator {
      margin: 0;
      padding: 0;
      list-style: none;
      display: flex;
      align-items: center;
    }

    .slider-indicator li {
      width: 8px;
      height: 8px;
      margin: 4px;
      border-radius: 50%;
      background: #fff;
      opacity: 0.4;
      cursor: pointer;
    }

    .slider-indicator li.active {
      width: 12px;
      height: 12px;
      opacity: 1;
    }
  </style>
</head>

<body>
  <div class="slider">
    <div class="slider-wrapper">
      <img src="./images/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
      <p>对人类来说会不会太超前了？</p>
      <ul class="slider-indicator">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
      </div>
    </div>
  </div>
  <script>
    // 1. 初始数据
    const sliderData = [
      { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
      { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
      { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
      { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
      { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
      { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
      { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
      { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
    ]
    const randomIndex = Math.floor(Math.random() * sliderData.length);
    const data = sliderData[randomIndex];
    const sliderIndicator = document.querySelector('.slider-indicator');
    const sliderImg = document.querySelector('.slider-wrapper img');
    const sliderDisc = document.querySelector('.slider-footer p')
    const sliderFooter = document.querySelector('.slider-footer');
    const active = document.querySelector('.active');
    active.classList.remove('active');
    sliderIndicator.children[randomIndex].classList.add('active');
    sliderImg.src = data.url;
    sliderDisc.innerText = data.title;
    sliderFooter.style.backgroundColor = data.color;
  </script>
</body>

</html>
```



## 操作表单元素属性

- 表单很多情况，也需要修改属性，比如点击眼睛，可以看到密码，本质是把表单类型转换为文本框
- 正常的有属性有取值的，跟其他的标签属性没有任何区别
- 获取：DOM对象.属性名
- 设置：DOM对象.属性 = 新值

![img](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/SNAGHTML69e4b4.PNG)

- 表单属性中添加就有效果，移除就没有效果，一律用布尔值表示，如果为 `true` 代表添加了该属性，如果是 `false` 代表移除了该属性

- 比如：disabled、checked、selected

  示例1：

  ```javascript
  <input type="checkbox">
  <script>
      // 1. 获取 
      const input = 	document.querySelector('input');
  	console.log(input.checked); // false，只接受布尔值
  	input.checked = 'true'; // 'true' 表示 true，会选中，不提倡 
  </script>
  ```

  示例2：

  ```javascript
  <button>点击</button>
  <script>
      // 1. 获取
      const btn = document.querySelector('button');
  	console.log(btn.disabled); // 默认 false 不禁用
  	btn.disabled = true; // 禁用按钮
  </script>
  ```

  

## 自定义属性

- **标准属性：**标签天生自带的属性，比如class id title等，可以直接使用点语法操作，比如：disabled、checked、selected

- **自定义属性：**

  - 在 HTML5 中推出来了专门的 `data-` 自定义属性
  - 在标签一律以 `data-` 开头
  - 在 DOM 对象上一律以 dataset 对象方式获取

  示例：

  ```javascript
  <div data-id="1" data-spm="something">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>
  <script>
      const one = document.querySelector('div');
  	console.log(one.dataset.id); // 1
  	console.log(one.dataset.spm); // something
  </script>
  ```

  

  

  # 定时器-间歇函数

> 目标：能够说出定时器函数在开发中的使用场景

- 网页中经常会需要一种功能：每隔一段时间需要<font color='red'>自动</font>执行一段代码，不需要我们手动去触发
- 例如：网页中的倒计时
- 要实现这种需求，需要定时器函数
- 定时器函数有两种，今天我们讲间歇函数



> 目标：能够使用定时器函数重复执行代码

定时器函数可以开启和关闭定时器

## 1. 开启定时器

```javascript
setIneterval(函数，间隔时间)
```

- 作用：每隔一段时间调用这个函数
- 间隔时间单位是毫秒



> [!tip]
>
> 举例说明
>
> ```javascript
> function fn() { 
>     console.log('一秒执行一次');
> }
> // setInterval(函数, 间隔时间) // 函数名不要加小括号
> 
> // 每隔一秒调用 repeat 函数
> let n = setInterval(fn, 3000); 
> ```
>
> 注意
>
> 1. 函数名字<font color='red'>不需要加括号</font>
> 2. 定时器返回的是一个 id 数字

定时器函数可以开启和关闭定时器



## 2. 关闭定时器

```javascript
let 变量名 = setInterval(函数, 间隔时间)
clearInterval(变量名)
```

一般不会刚创建就停止，而是满足一定条件再停止

```
let timer = setInterval(function() {
	console.log('hi~~');
}, 1000);
clearInterval(timer)
```

> [!attention]
>
> 注意
>
> 1. 函数名字<font color='red'>不需要加括号</font>
> 2. <font color='red'>定时器返回的是一个 id 数字</font>



## 总结

1. 定时器函数有什么作用？

   - 可以根据时间自动重复执行某些代码

2. 定时器函数如何开启？

   - setInterval(函数名，时间)

3. 定时器如何关闭

   ```javascript
   let 变量名 = setInterval(函数, 间隔时间)
   clearInterval(变量名)
   ```



## 案例：阅读注册协议

需求：按钮60秒之后才可以使用

分析：

1. 开始先把按钮禁用（disabled 属性）
2. 一定要获取元素
3. 函数内处理逻辑
   1. 描述开始减减
   2. 按钮里面的文字跟着一起变化
   3. 如果秒数等于 0 停止定时器，里面文字变为同意，最后，按钮可以点击

效果图：

![img](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/SNAGHTMLffd96f.PNG)

参考代码：

```javascript
 <textarea name="" id="" cols="30" rows="10">
        用户注册协议
        欢迎注册成为京东用户！在您注册过程中，您需要完成我们的注册流程并通过点击同意的形式在线签署以下协议，请您务必仔细阅读、充分理解协议中的条款内容后再点击同意（尤其是以粗体或下划线标识的条款，因为这些条款可能会明确您应履行的义务或对您的权利有所限制）。
        【请您注意】如果您不同意以下协议全部或任何条款约定，请您停止注册。您停止注册后将仅可以浏览我们的商品信息但无法享受我们的产品或服务。如您按照注册流程提示填写信息，阅读并点击同意上述协议且完成全部注册流程后，即表示您已充分阅读、理解并接受协议的全部内容，并表明您同意我们可以依据协议内容来处理您的个人信息，并同意我们将您的订单信息共享给为完成此订单所必须的第三方合作方（详情查看
    </textarea>
    <br>
    <button class="btn">我已经阅读用户协议(60)</button>
    <script>
        const btnDom = document.querySelector('.btn');
        btnDom.disabled = true;
        let i = 60;
        let n = setInterval(function() {
            i--;
            btnDom.innerText = `我已经阅读用户协议(${i})`;
            if (i <= 0) {
                btnDom.innerText = '同意';
                btnDom.disabled = false;
                clearInterval(n);
            }
        }, 1000);
    </script>
```

:point_right: [演示demo](https://weixinao.github.io/Web-Study-Source-Code/webAPIs第一天/02-code/17_用户注册倒计时.html)

# 时间监听（绑定）

## 事件监听

目标：能够给 DOM 元素添加事件监听

- 什么是事件？

  事件是在编程时系统内发生的<font color='red' style="font-weight: bold;">动作</font>或者发生的事情

  比如用户在网页上<font color='red' style="font-weight: bold;">单击</font>一个按钮

- **什么是事件监听？**

  就是让程序检测是否有事件发生，一旦有事件触发，就立即调用一个函数做出响应，也称为「绑定事件」或「注册事件」

  比如鼠标经过显示下拉菜单，比如点击可以播放轮播图等等

- 语法：

  ```javascript
  元素对象.addEventListener('事件类型', 要执行的函数)
  ```

- 事件监听三要素

  - **时间源**：那个 dom 元素被事件触发了，要获取 dom 元素
  - **事件类型**：用什么方式触发，比如鼠标单击 click、鼠标经过 mouseover 等
  - **事件调用的函数：**要做什么事

  > [!attention]
  >
  > 「要执行的函数」不会立即执行，只有事件触发时，才会执行



## 总结

1. 什么是事件监听？
   - 就是让程序检测是否有事件产生，一旦有事件触发，就立即调用一个函数做出响应，也称为「注册事件」
2. 事件监听三要素是什么？
   - 事件源（谁被触发了）
   - 事件类型（用什么方式触发，点击还是鼠标经过）
   - 事件处理程序（要做什么事情）



## 案例：随机点名案例

![2023-02-26_19-41-46](https://raw.githubusercontent.com/WeiXinao/imgBed2/main/img/2023-02-26_19-41-46.gif)

业务分析：

1. 点击开始按钮随机抽取数组的一个数据，放到页面中
2. 点击结束按钮删除数组当前抽取的一个数据
3. 当抽取到最后一个数据的时候，两个按钮同时禁用（写点开始里面，只剩最后一个数据不用抽了）

核心：利用定时器快速展示，停止定时器结束展示

```html
<head>
	<style>
        * {
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .box {
            width: 600px;
            margin: 50px auto;
            display: flex;
            font-size: 25px;
            line-height: 40px;
        }

        .qs {

            width: 450px;
            height: 40px;
            color: red;

        }

        .btns {
            text-align: center;
        }

        .btns button {
            width: 120px;
            height: 35px;
            margin: 0 50px;
        }
    </style>
</head>

<body>
    <h2>随机点名</h2>
    <div class="box">
        <span>名字是：</span>
        <div class="qs">这里显示姓名</div>
    </div>
    <div class="btns">
        <button class="start">开始</button>
        <button class="end">结束</button>
    </div>

    <script>
        // 数据数组
        const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']
        const start = document.querySelector('.start');
        const end = document.querySelector('.end');
        const name = document.querySelector('.qs');
        let randomIndex;
        let intervalIndex;
		start.addEventListener('click', () => {
            start.disabled = true;
            end.disabled = false;
            intervalIndex = setInterval(() => {
                randomIndex = Math.floor(Math.random() * arr.length);
                name.innerText = arr[randomIndex];
            }, 100)    
            if (arr.length === 1) {
                start.disabled = end.disabled = true;
            }
        })
        end.addEventListener('click', () => {
            end.disabled = true;
            start.disabled = false;
            clearInterval(intervalIndex);
            arr.splice(randomIndex, 1);
        })
    </script>
</body>
```

## 解惑：垃圾回收机制

```html
<button>点击</button>
<script>
    const btn = document.querySelector('button');
	btn.addEventListener('click', () => { 
    const num = Math.random();
    console.log(num);
})
</script>
```

阅读上面代码，我们会发现，用 const 声明的变量，不是不允许多次声明吗？但是，每当我们点击一下按钮，事件回调函数就会调用一次，就会声明一个新的 num，多次声明 num 为什么没有报错？

```
答：其实，它用到了我们的垃圾回收机制，如果一个函数中的变量，如果我们执行完一个函数之后，如果这个变量不再使用了，它就会被回收掉。
当
```





