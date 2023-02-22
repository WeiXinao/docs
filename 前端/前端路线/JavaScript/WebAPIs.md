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

👉 多参看文档：