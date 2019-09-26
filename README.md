# Event
本章内容事件相关：理解事件流、使用事件处理程序、事件对象、不同的事件类型等。<br/>
JavaScript 与 HTML 之间的交互是通过事件实现的。

## 事件流
* **事件冒泡**(所有浏览器都支持)：IE 的事件流，逐级向上传播直至传播到 document 对象，div->body->html->document
* **事件捕获**(IE<9不支持)：从不具有节点到具体节点，document->html->body->div，尽管“DOM2 级事件”规范要求事件应该从 document 对象开始传播，但这些浏览器都是从 window 对象开始捕获事件的。
* **DOM事件流**：三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。

## 事件处理程序
事件就是用户或浏览器自身执行的某种动作。诸如 click、load 和 mouseover，都是事件的名字。而响应某个事件的函数就叫做事件处理程序（或事件侦听器）。事件处理程序的名字以"on"开头，因此click 事件的事件处理程序就是 onclick。

| 名称 |HTML事件处理程序|DOM0级事件处理程序| DOM2级事件处理程序 | IE事件处理程序 |
| --- | --- | --- | --- | --- |
| description |在html上绑定的与相应事件处理程序同名的 HTML 特性  | 通过JS指定事件处理程序方式，就是将一个函数赋值给一个事件处理程序属性 |这两个方法接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。true，表示在捕获阶段调用事件处理程序；false，表示在冒泡阶段调用事件处理程序  | 这两个方法接受两个参数：事件处理程序名称与事件处理程序函数。由于IE8及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段 |
| 绑定事件处理程序 | \<input type="button" onclick="showMsg()"\/> | btn.onclick=function(){} | btn.addEventListener( "click",handle,false) | btn.attachEvent( "onclick",handle) |
| 删除事件处理程序 | - | btn.onclick=null | btn.removeEventListen er("click",handle,false)|btn.detachEvent( "onclick",handle) |
| this指向 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 在全局作用域中运行 |
| 能否添加多个事件处理程序 | 不可以 | 不可以 | 可以 | 可以 |
| 事件对象 | event参数 | event参数（非IE）window.event（IE） | event参数 | event参数 或 window.event |

## 事件对象
只有在事件处理程序执行期间，event 对象才会存在；一旦事件处理程序执行完
成，event 对象就会被销毁。同时，触发的事件类型不一样，事件对象可用的属性和方法也不一样。不过，所有事件都会有下表列出的成员：
##### DOM中的事件对象（以下都是只读的）：
* **type**：被触发的事件的类型。eg：click
* **target**：事件的目标。点击谁就是那个元素。
* **currentTarget**：绑定事件处理程序那个元素。在事件处理程序内部（非IE），this===currentTarget的值
* **eventPhase**：事件流位于哪个阶段阶段：1表示捕获阶段，2表示“处于目标”，3表示冒泡阶段。
* **bubbles**：表明事件是否冒泡。
* **stopPropagation**()：取消事件的进一步捕获或冒泡。如果bubbles为true，则可以使用这个方法。
* **stopImmediatePropagation**()：和stopPropagation相比，同样可以阻止事件的进一步捕获或冒泡，不同点在于其还可以把这个元素绑定的同类型事件也阻止了。
* **cancelable**：表明是否可以取消事件的默认行为。
* **defaultPrevented**：为 true 表示已经调用了 preventDefault()
* **preventDefault**()：取消事件的默认行为。如果cancelable是 true，则可以使用这个方法。eg：a标签默认跳转行为
* **detail**：与事件相关的细节信息
* **trusted**：为true表示事件是浏览器生成的。为false表示事件是由开发人员通过JS创建
* **view**：与事件关联的抽象视图。等同于发生事件的window对象
##### IE中的事件对象：
* **type**：被触发的事件的类型。
* **srcElement**：事件的目标（与DOM中的target属性相同）
* **cancelBubble**：默认值为false，但将其设置为true就可以取消事件冒泡（与DOM中的stopPropagation()方法的作用相同）。由于 IE 不支持事件捕获，因而只能取消事件冒泡。
* **returnValue**：默认值为true，但将其设置为false就可以取消事件的默认行为（与
DOM中的preventDefault()方法的作用相同）

## 事件类型

