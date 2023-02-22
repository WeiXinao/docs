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
	- 作用：<font color=#F36208>文档树直观的体现了标签与标签之间的关系</font>
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

>[!NOTE]
>注意事项：
>
>哪怕只有一个元素，通过 `querySelectAll()` 获取过来的也是一个<font color=#F36208 style="font-weight: bold">伪数组</font>，
>里面只有一个元素而已

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

👉[演示demo]()

### 总结

1. 获取页面中的标签我们最终常用哪两种方式？
	- `querySelectorAll()`
	- `querySelector()`
2. 它们两者的区别是什么？
	- `querySelector()` 只能选择一个元素，可以直接操作
	- `querySelectorAll()` 可以选择多个元素，得到的是伪数组，需要遍历得到每一个元素
1. 他们两者小括号里面的参数有什么注意事项？
	- 里面写 CSS 选择器 
	- <font color=#F36208 style="font-weight: bold;">必须是字符串，也就是必须加引号</font>

# 操作元素内容

- 对象.innerText 属性
- 对象.innerHTML 属性

1. **元素.innerText 属性**
	- 将文本内容添加/更新到任意标签位置
	- 显示纯文本，不解析标签

>[!NOTE]
>举例说明：
>
>```javascript
>		 // 1. 获取元素
 >       const box = document.querySelector('.box');
 >       // 2. 修改文字内容 对象.innerText 属性
 >       console.log(box.innerText); // 获取文字内容
 >       box.innerText = '<strong>我是一个盒子</strong>';  // 修改文字内容
>```


### 2. 元素.innerHTML 属性

- 将文本内容添加/更新到任意标签位置
- 会解析标签，多标签建议使用模板字符

>[!NOTE]
>举例说明
>```javascript
> 	// 3. innerHTML 解析标签
>     console.log(box.innerHTML);
>     box.innerHTML = '我要更换';
>     box.innerHTML = '<strong>我要更换</strong>'
>```

### 总结













