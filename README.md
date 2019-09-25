# Event
本章内容事件相关：理解事件流、使用事件处理程序、事件对象、不同的事件类型等。<br/>
JavaScript 与 HTML 之间的交互是通过事件实现的。

## 事件流
* **事件冒泡**(所有浏览器都支持)：IE 的事件流，逐级向上传播直至传播到 document 对象，div->body->html->document
* **事件捕获**(IE<9不支持)：从不具有节点到具体节点，document->html->body->div，尽管“DOM2 级事件”规范要求事件应该从 document 对象开始传播，但这些浏览器都是从 window 对象开始捕获事件的。
* **DOM事件流**：三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。

## 事件处理程序
事件就是用户或浏览器自身执行的某种动作。诸如 click、load 和 mouseover，都是事件的名字。而响应某个事件的函数就叫做事件处理程序（或事件侦听器）。事件处理程序的名字以"on"开头，因此click 事件的事件处理程序就是 onclick。

| 事件处理程序 |HTML事件处理程序|DOM0级事件处理程序| DOM2级事件处理程序 | IE事件处理程序 |
| --- | --- | --- | --- | --- |
| description |在html上绑定的与相应事件处理程序同名的 HTML 特性  | 通过JS指定事件处理程序方式，就是将一个函数赋值给一个事件处理程序属性 |这两个方法接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。true，表示在捕获阶段调用事件处理程序；false，表示在冒泡阶段调用事件处理程序  | 这两个方法接受两个参数：事件处理程序名称与事件处理程序函数。由于IE8及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段 |
| 能否添加多个事件处理程序 | 不可以 | 不可以 | 可以 | 可以 |
| 绑定事件处理程序 | <input type="button" onclick="showMessage()"/>  | btn.onclick=function(){} | btn.addEventListener("click",handle,false) | btn.attachEvent("onclick",handle) |
| 删除事件处理程序 | - | btn.onclick=null | btn.removeEventListener("click",handle,false)|btn.detachEvent("onclick",handle) |
| this指向 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 在全局作用域中运行 |
