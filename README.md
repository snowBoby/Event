# Event
本章内容事件相关：理解事件流、使用事件处理程序、事件对象、不同的事件类型等。<br/>
JavaScript 与 HTML 之间的交互是通过事件实现的。

## 1事件流
* **事件冒泡**(所有浏览器都支持)：IE 的事件流，逐级向上传播直至传播到 document 对象，div->body->html->document
* **事件捕获**(IE<9不支持)：从不具有节点到具体节点，document->html->body->div，尽管“DOM2 级事件”规范要求事件应该从 document 对象开始传播，但这些浏览器都是从 window 对象开始捕获事件的。
* **DOM事件流**：三个阶段，事件捕获阶段、处于目标阶段和事件冒泡阶段。

## 2事件处理程序
事件就是用户或浏览器自身执行的某种动作。诸如 click、load 和 mouseover，都是事件的名字。而响应某个事件的函数就叫做事件处理程序（或事件侦听器）。事件处理程序的名字以"on"开头，因此click 事件的事件处理程序就是 onclick。

| 名称 |HTML事件处理程序|DOM0级事件处理程序| DOM2级事件处理程序 | IE事件处理程序 |
| --- | --- | --- | --- | --- |
| description |在html上绑定的与相应事件处理程序同名的 HTML 特性  | 通过JS指定事件处理程序方式，就是将一个函数赋值给一个事件处理程序属性 |这两个方法接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。true，表示在捕获阶段调用事件处理程序；false，表示在冒泡阶段调用事件处理程序  | 这两个方法接受两个参数：事件处理程序名称与事件处理程序函数。由于IE8及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段 |
| 绑定事件处理程序 | \<input type="button" onclick="showMsg()"\/> | btn.onclick=function(){} | btn.addEventListener( "click",handle,false) | btn.attachEvent( "onclick",handle) |
| 删除事件处理程序 | - | btn.onclick=null | btn.removeEventListen er("click",handle,false)|btn.detachEvent( "onclick",handle) |
| this指向 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 所属元素的作用域内运行 | 在全局作用域中运行 |
| 能否添加多个事件处理程序 | 不可以 | 不可以 | 可以 | 可以 |
| 事件对象 | event参数 | event参数（非IE）window.event（IE） | event参数 | event参数 或 window.event |

## 3事件对象
只有在事件处理程序执行期间，event 对象才会存在；一旦事件处理程序执行完
成，event 对象就会被销毁。同时，触发的事件类型不一样，事件对象可用的属性和方法也不一样。不过，所有事件都会有下表列出的成员：
### 3.1DOM中的事件对象（以下都是只读的）：
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
### 3.2IE中的事件对象：
* **type**：被触发的事件的类型。
* **srcElement**：事件的目标（与DOM中的target属性相同）
* **cancelBubble**：默认值为false，但将其设置为true就可以取消事件冒泡（与DOM中的stopPropagation()方法的作用相同）。由于 IE 不支持事件捕获，因而只能取消事件冒泡。
* **returnValue**：默认值为true，但将其设置为false就可以取消事件的默认行为（与
DOM中的preventDefault()方法的作用相同）

## 4事件类型
* **UI 事件**：当用户与页面上的元素交互时触发；DOMContentedLoad->load->pageShow->pageHide->beforeUnload->unload
* **焦点事件**：当元素获得或失去焦点时触发；当焦点从页面中的一个元素移动到另一个元素，事件顺序：blur->focusout->DOMFocusOut->focus->focusin->DOMFocusIn
* **鼠标事件**：当用户通过鼠标在页面上执行操作时触发；click、dbclick、mousedown、mouseup、mouseenter、mouseleave、mouseover、mouseout、mousemove
* **滚轮事件**：当使用鼠标滚轮（或类似设备）时触发；
* **文本事件**：当在文档中输入文本时触发；
* **键盘事件**：当用户通过键盘在页面上执行操作时触发；
* **合成事件**：当为 IME（Input Method Editor，输入法编辑器输入字符时触发；
* **变动事件**：当底层 DOM 结构发生变化时触发。

### 4.1 UI 事件
这个 event 对象没有任何附加信息，但在兼容 DOM 的浏览器中，event.target 属性的值会被设置为document。有两种方式：1、通过JS来指定事件处理程序；2、通过HTML在<body>元素中通过相应的特性来指定（因为在HTML中无法访问window元素）。根据“DOM2 级事件”规范，应该在 document 而非 window 上面触发 load 事件。但是，所有浏览器都在 window 上面实现了该事件，以确保向后兼容。
* **load**：当页面完全加载后（包括所有图像、JavaScript 文件、CSS 文件等外部资源后）在 window 上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在<img>元素上面触发（**新图像元素不一定要从添加到文档后才开始下载，只要设置了 src 属性就会开始下载，事件处理程序要在src之前绑定，new Image()创建的图片无法将其添加到 DOM 树中，可以做图片预加载，createElement可以添加DOM树上**），或者当嵌入的内容加载完毕时在<object>（ **<script>（IE<=8不支持script的onload）、<link>（只有IE和Opera支持）等，与图像不同，只有在设置了src 属性并将该元素添加到文档后，才会开始下载文件**）元素上面触发。
* **unload**：当页面完全卸载后在 window 上面触发（**一般用于清除引用，以避免内存泄漏**），当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在<object>元素上面触发。
* **resize**：当窗口或框架的大小变化时在 window 或框架上面触发。（**不要在这个事件的处理程序中加入大计算量的代码，因为这些代码有可能被频繁执行，从而导致浏览器反应明显变慢**）
* **scroll**：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。<body>元素中包含所加载页面的滚动条。（**与 resize 事件类似，scroll 事件也会在文档被滚动期间重复被触发，所以有必要尽量保持事件处理程序的代码简单**）
* **abort**：在用户停止下载过程时，如果嵌入的内容没有加载完，则在<object>元素上面触发。
* **error**：当发生 JavaScript 错误时在 window 上面触发，当无法加载图像时在<img>元素上面触发，当无法加载嵌入内容时在<object>元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。
* **select**：当用户选择文本框（<input>或<texterea>）中的一或多个字符时触发。
### 4.2焦点事件
利用这些事件并与 document.hasFocus()方法及document.activeElement 属性配合，可以知晓用户在页面上的行踪。
* **blur**：在元素失去焦点时触发。这个事件**不会冒泡**；
* **focus**：在元素获得焦点时触发。这个事件**不会冒泡**；
* **focusin/DOMFocusIn**：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。有兼容性问题
* **focusout/DOMFocusOut**：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。有兼容性问题
### 4.3鼠标事件
除了 **mouseenter** 和 **mouseleave**，所有鼠标事件都会**冒泡**。
* **click**：在用户单击主鼠标按钮（一般是左边的按钮）或者按下回车键时触发。只有onclick 事件处理程序既可以通过键盘也可以通过鼠标执行。**下面几个只能通过鼠标触发这个事件**。（**只有在同一个元素上相继触发 mousedown 和 mouseup 事件，才会触发 click 事件，mousedown->mouseup->click**）
* **dblclick**：在用户双击主鼠标按钮（一般是左边的按钮）时触发。（**mousedown->mouseup->click->mousedown->mouseup->click->dbclick**）
* **mousedown**：在用户按下了任意鼠标按钮时触发。
* **mouseup**：在用户释放鼠标按钮时触发。
* **mouseenter**：在鼠标光标从元素外部首次移动到元素范围之内时触发。这个事件**不冒泡**
* **mouseleave**：在位于元素上方的鼠标光标移动到元素范围之外时触发。这个事件**不冒泡**
* **mousemove**：当鼠标首次移入元素内会触发，同时在其子元素移入移除都会触发。
* **mouseover**：在鼠标移除该元素会触发，同时在其子元素移入移除都会触发。

鼠标事件特有的事件对象属性：
* **clientX**：表示事件发生时鼠标指针在**视口**中的水平位置。
* **clientY**：表示事件发生时鼠标指针在**视口**中的垂直位置。
* **pageX**：表示事件发生时鼠标指针在**页面**中的水平位置。
* **pageY**：表示事件发生时鼠标指针在**页面**中的垂直位置。
* **screenX**：相对于**整个电脑屏幕**的水平位置。
* **screenY**：相对于**整个电脑屏幕**的垂直位置。
* **shiftKey/ctrlKey/altKey/metaKey(ie<9不支持metaKey)** ：修改键布尔值，虽然鼠标事件主要是使用鼠标来触发的，但在按下鼠标时键盘上的某些键的状态也可以影响到所要采取的操作。这些修改键就是 Shift、Ctrl、Alt 和 Meta（在 Windows 键盘中是 Windows 键，在mac中是 Cmd 键）
* **relatedTarget/fromElement（ie<9的mouseover事件）/toElement（ie<9的mouseout事件）**：相关元素（**这个属性只对于 mouseover和mouseout事件才包含值；对于其他事件，这个属性的值是null**）。从当前元素转移到另外一个元素，当前元素或另外一个元素就是相关元素，要看是哪个事件类型。

**mousedown** 和 **mouseup**事件，除了有以上事件对象属性之外，还有下面特殊属性：
* **button**：0表示主鼠标按钮，1表示中间的鼠标按钮（鼠标滚轮按钮）2表示次鼠标按钮。

**IE** 也通过下列属性为**鼠标事件**提供了更多信息:
* **offsetX**：相对于**目标元素**的水平位置。目标元素就是target点击元素本身。
* **offsetY**：相对于**目标元素**的垂直位置。
* **shiftLeft/ctrlLeft/altLeft**：布尔值，表示是否按下了 Shift/Ctrl/Alt 键。如果shiftLeft/ctrlLeft/altLeft的值为true，则对应shiftKey/ctrlKey/altKey的值也为 true。


`*注意*：1、在页面没有滚动的情况下，pageX 和 pageY 的值与 clientX 和 clientY 的值相等；IE8 及更早版本不支持事件对象上的页面坐标，但可以通过计算得到 pageX = event.clientX + (document.body.scrollLeft ||
 document.documentElement.scrollLeft)
 2、iOS 和 Android 设备的实现非常特别，因为这些设备没有鼠标。在面向 iPhone 和 iPod 中的 Safari开发时，要记住以下几点。
 不支持 dblclick 事件。双击浏览器窗口会放大画面，而且没有办法改变该行为。
轻击可单击元素会触发 mousemove 事件。如果此操作会导致内容变化，将不再有其他事件发生；如果屏幕没有因此变化，那么会依次发生 mousedown、mouseup 和 click 事件。轻击不可单击的元素不会触发任何事件。可单击的元素是指那些单击可产生默认操作的元素（如链接），或者那些已经被指定了 onclick 事件处理程序的元素。
mousemove 事件也会触发 mouseover 和 mouseout 事件。
两个手指放在屏幕上且页面随手指移动而滚动时会触发 mousewheel 和 scroll 事件。`
### 4.4滚轮事件
* **mousewheel/DOMMouseScroll（Firefox）**：这个事件跟踪鼠标滚轮，类似于 Mac 的触控板。

滚轮事件的事件对象属性除包含**鼠标事件**的所有标准信息外，mousewheel还包含一个特殊的 wheelDelta 属性，而DOMMouseScroll将滚轮信息保存在detail属性中：

* wheelDelta：滚动的倍数。
