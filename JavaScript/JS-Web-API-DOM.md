# JS-Web-API-DOM

DOM作为DOM树结构，可进行节点操作，结构操作；并且得避免重复插入多次DOM节点，影响性能。

## 1. attr 和 property的区别？

- `property`：修改对象属性，不会体现在HTML结构中

- `attribute`： 修改HTML属性，会改变HTML结构

- 两者都有可能引起DOM重新渲染

## 2. 一次性插入多个DOM节点，考虑性能？

DOM操作非常“昂贵”，应避免频繁的DOM操作

- 可以对DOM查询做缓存
- 或者将频繁操作改为一次性操作 通过DOM的`createDocumentFragment()`方法创建文档片段，文档片段不插入DOM结构，多次插入文档片段后，一次性插入。

```js
// 缓存DOM查询结果

// 不缓存查询例子
for (let i = 0; i < document.getElementsByTagName('p').length; i++) {
    // 每次循环，都会计算length，频繁进行DOM查询
}

// 缓存DOM查询结果
const pList = document.getElementsByTagName('p')
const length = pList.length
for (let i = 0; i < length; i++) {
	// 缓存length，只进行一次DOM查询
}
```



```js
// 将频繁操作改为一次性操作

const list = document.getElementById('list')

// 创建一个文档片段，此时还没有插入到 DOM 结构中
const frag = document.createDocumentFragment()

for (let i  = 0; i < 20; i++) {
    const li = document.createElement('li')
    li.innerHTML = `List item ${i}`

    // 先插入文档片段中
    frag.appendChild(li)
}

// 都完成之后，再统一插入到 DOM 结构中
list.appendChild(frag)

console.log(list)
```



