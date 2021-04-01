# JS-Web-API-事件

## 1. 编写一个通用的事件监听函数

```js
/**
* params: elem(绑定的DOM元素) type(绑定事件种类) selector(需要代理绑定的DOM元素) fn(事件处理函数)
*
*/
function bindEvent(elem, type, selector, fn) {
    if (fn == null) {
        fn = selector
        selector = null
    }
    elem.addEventListener(type, event => {
        const target = event.target
        if (selector) {
            // 代理绑定
            if (target.matches(selector)) {
                fn.call(target, event)
            }
        } else {
            // 普通绑定
            fn(event)
        }
    })
}
```

## 2. 描述事件冒泡流程

- 基于DOM树结构
- 事件会顺着触发元素向上冒泡执行

## 3. 无线下拉的图片列表，如何监听每个图片的点击？

使用事件代理。

```js
const div3 = document.getElementById('div3')
bindEvent(div3, 'click', 'a', function (event) {
    event.preventDefault() // 阻止默认行为
    alert(this.innerHTML)
})
```



