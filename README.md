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

* **UI 事件**：当用户与页面上的元素交互时触发；DOMContentedLoad->load->pageShow->beforeUnload->pageHide->unload、hashchange
* **焦点事件**：当元素获得或失去焦点时触发；当焦点从页面中的一个元素移动到另一个元素，事件顺序：blur->focusout->DOMFocusOut->focus->focusin->DOMFocusIn
* **鼠标事件**：当用户通过鼠标在页面上执行操作时触发；click、dbclick、mousedown、mouseup、mouseenter、mouseleave、mouseover、mouseout、mousemove、contextmenu（h5事件）
* **滚轮事件**：当使用鼠标滚轮（或类似设备）时触发；mousewheel、DOMMouseScroll
* **键盘事件**：当用户通过键盘在页面上执行操作时触发；keydown->keypress->textInput（文本事件）->keyup
* **文本事件**：当在文档中输入文本时触发；textInput
* **复合事件**：当为 IME（Input Method Editor，输入法编辑器输入字符时触发；compositionstart、compositionupdate、compositionend
* **变动事件**：当底层 DOM 结构发生变化时触发。
* **设备事件**：可以让开发人员确定智能手机和平板电脑的用户在怎样使用设备。
* **触摸事件**：会在用户手指放在屏幕上面时、在屏幕上滑动时或从屏幕上移开时触发。
* **手势事件**：当两个手指触摸屏幕时就会产生手势，手势通常会改变显示项的大小，或者旋转显示项。gesturestart->touchstart,gestureend->touchend
* **表单事件**：除了支持鼠标、键盘、更改和 HTML 事件之外，所有表单字段都支持focus、blur、change
* **拖放事件**：拖动某元素时，在目标元素（被拖动的元素）上将依次触发dragstart->drag->dragend，当某个元素被拖动到一个有效的放置目标上时，在目标元素上将依次触发dragenter->dragover->dragleave/drop

### 4.1UI 事件
这个 event 对象没有任何附加信息，但在兼容 DOM 的浏览器中，event.target 属性的值会被设置为document。有两种方式：1、通过JS来指定事件处理程序；2、通过HTML在<body>元素中通过相应的特性来指定（因为在HTML中无法访问window元素）。根据“DOM2 级事件”规范，应该在 document 而非 window 上面触发 load 事件。但是，所有浏览器都在 window 上面实现了该事件，以确保向后兼容
* **DOMContentLoaded**：在形成完整的 DOM 树之后就会触发。不理会图像、JS 文件、CSS 文件或其他资源是否已经下载完毕。
* **load**：当页面完全加载后（包括所有图像、JavaScript 文件、CSS 文件等外部资源后）在 window 上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在<img>元素上面触发（**新图像元素不一定要从添加到文档后才开始下载，只要设置了 src 属性就会开始下载，事件处理程序要在src之前绑定，new Image()创建的图片无法将其添加到 DOM 树中，可以做图片预加载，createElement可以添加DOM树上**），或者当嵌入的内容加载完毕时在元素（ **<script>（IE<=8不支持script的onload）、<link>（只有IE和Opera支持）等，与图像不同，只有在设置了src 属性并将该元素添加到文档后，才会开始下载文件**）元素上面触发。
* **pageshow**：在页面显示时触发，无论该页面是否来自 bfcache。在重新加载的页面中，pageshow 会在 load 事件触发后触发；另外要注意的是，虽然这个事件的目标是 document，但必须将其事件处理程序添加到 window。
* **pagehide**：会在浏览器卸载页面的时候触发，而且是在unload 事件之前触发。与 pageshow 事件一样，pagehide 在 document 上面触发，但其事件处理程序必须要添加到 window 对象。
* **beforeunload**：会在浏览器卸载页面之前触发，可以通过它来取消卸载并继续使用原有页面。离不离开当前页面控制器必须交给用户，出现弹窗让用户自己选择。为了显示这个弹出对话框，必须将 event.returnValue 的值设置为要显示给用户的字符串（对IE 及 Fiefox 而言），同时作为函数的值返回（对 Safari 和 Chrome 而言）
* **unload**：当页面完全卸载后在 window 上面触发（**一般用于清除引用，以避免内存泄漏**），当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在元素上面触发。
* **hashchange**：在 URL 的参数列表（及 URL 中“#”号后面的所有字符串）发生变化时通知开发人员。必须要把 hashchange 事件处理程序添加给 window 对象。
* **resize**：当窗口或框架的大小变化时在 window 或框架上面触发。（**不要在这个事件的处理程序中加入大计算量的代码，因为这些代码有可能被频繁执行，从而导致浏览器反应明显变慢**）
* **scroll**：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。<body>元素中包含所加载页面的滚动条。（**与 resize 事件类似，scroll 事件也会在文档被滚动期间重复被触发，所以有必要尽量保持事件处理程序的代码简单**）
* **abort**：在用户停止下载过程时，如果嵌入的内容没有加载完，则在元素上面触发。
* **error**：当发生 JavaScript 错误时在 window 上面触发，当无法加载图像时在<img>元素上面触发，当无法加载嵌入内容时在元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。
* **select**：与 select()方法对应的，当用户选择文本框（<input>或<texterea>）中的一或多个字符时触发。特有的事件对象的属性如下：
    * selectionStart：选择的字符开始位置。
    * selectionEnd：选择的字符结束位置。但是ie9以下版本不支持可以通过document.selection.createRange().text
* **readystatechange**：提供与文档或元素的加载状态有关的信息，支持readystatechange 事件的每个对象都有一个 readyState 属性，可能有以下5个值中的一个。uninitialized（未初始化，对象存在但尚未初始化）、loading（正在加载，对象正在加载数据）、loaded（加载完毕，对象加载数据完成）、interactive（交互，可以操作对象了，但还没有完全加载）、complete（完成，对象已经加载完毕）；可以用来确定外部的 JavaScript 和 CSS 文件是否已经加载完成，readyState 属性无论等于 "loaded" 还是"complete"都可以表示资源已经可用。
 
  #### pageshow和pagehide事件特有的事件对象属性：
  * **persisted**：如果页面被保存在了 bfcache 中，则这个属性的值为 true；否则，这个属性的值为 false；对于 pageshow事件，如果页面是从 bfcache 中加载的，那么 persisted 的值就是 true；对于 pagehide 事件，如果页面在卸载之后会被保存在 bfcache 中，那么 persisted 的值也会被设置为 true。因此，当第一次触发 pageshow 时，persisted 的值一定是 false，而在第一次触发 pagehide 时，persisted 就会变成 true（除非页面不会被保存在 bfcache 中）。
  `*知识扩展*：Firefox 和 Opera 有一个特性，名叫“往返缓存”（back-forward cache，或 bfcache），可以在用户使用浏览器的“后退”和“前进”按钮时加快页面的转换速度。这个缓存中不仅保存着页面数据，还保存了 DOM 和 JavaScript 的状态；实际上是将整个页面都保存在了内存里。如果页面位于 bfcache 中，那么再次打开该页面时就不会触发 load 事件。`
  #### hashchange事件特有的事件对象属性：
  * oldURL：("onhashchange" in window) && (document.documentMode ===undefined || document.documentMode > 7)判断支不支持
  * newURL：支持 hashchange 事件的浏览器有 IE8+、Firefox 3.6+、Safari 5+、Chrome 和 Opera 10.6+。在这些浏览器中，只有 Firefox 6+、Chrome 和 Opera 支持 oldURL 和 newURL 属性。为此，最好是使用 location对象来确定当前的参数列表
          
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
* **contextmenu**（h5事件）：点击非主鼠标按钮触发。可以自定义上下文菜单，可取消默认行为。

  #### 鼠标事件特有的事件对象属性：

  * **clientX**：表示事件发生时鼠标指针在**视口**中的水平位置。
  * **clientY**：表示事件发生时鼠标指针在**视口**中的垂直位置。
  * **pageX**：表示事件发生时鼠标指针在**页面**中的水平位置。
  * **pageY**：表示事件发生时鼠标指针在**页面**中的垂直位置。
  * **screenX**：相对于**整个电脑屏幕**的水平位置。
  * **screenY**：相对于**整个电脑屏幕**的垂直位置。
  * **shiftKey/ctrlKey/altKey/metaKey(ie<9不支持metaKey)** ：修改键布尔值，虽然鼠标事件主要是使用鼠标来触发的，但在按下鼠标时键盘上的某些键的状态也可以影响到所要采取的操作。这些修改键就是 Shift、Ctrl、Alt 和 Meta（在 Windows 键盘中是 Windows 键，在mac中是 Cmd 键）
  * **relatedTarget/fromElement（ie<9的mouseover事件）/toElement（ie<9的mouseout事件）**：相关元素（**这个属性只对于 mouseover和mouseout事件才包含值；对于其他事件，这个属性的值是null**）。从当前元素转移到另外一个元素，当前元素或另外一个元素就是相关元素，要看是哪个事件类型。

  #### **mousedown** 和 **mouseup**事件，除了有以上事件对象属性之外，还有下面特殊属性：
  * **button**：0表示主鼠标按钮，1表示中间的鼠标按钮（鼠标滚轮按钮）2表示次鼠标按钮。

  #### **IE** 也通过下列属性为**鼠标事件**提供了更多信息:
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

### 4.5键盘事件

* **keydown**：当用户按下键盘上的任意键时触发，而且如果按住不放的话，会重复触发此事件。在文本框发生变化之前被触发的；
* **keypress**：当用户按下键盘上的字符键（插入或删除字符）时触发，而且如果按住不放的话，会重复触发此事件。在文本框发生变化之前被触发的；按下 Esc 键也会触发这个事件。Safari 3.1 之前的版本也会在用户按下非字符键时触发 keypress事件。
* **keyup**：当用户释放键盘上的键时触发。在文本框已经发生变化之后被触发的;
  #### 键盘事件特有的事件对象属性：
  由于存在跨浏览器问题，因此不推荐使用 key、keyIdentifier 或 char，location或keyLocation。

  * **shiftKey/ctrlKey/altKey/metaKey(ie<9不支持metaKey)** ：键盘事件与鼠标事件一样，都支持相同的修改键；
  * **keyCode**：在发生 keydown 和 keyup 事件时，keyCode 属性的值与 ASCII 码中对应小写字母或数字的编码相同；
  * **charCode**：这个属性只有在发生keypress 事件时才包含值，按下的那个键所代表字符的 ASCII 编码。此时的 keyCode通常等于0或者也可能等于所按键的键码；在取得了字符编码之后，就可以使用 String.fromCharCode()将其转换成实际的字符
  * **key**：为了取代 keyCode，值就是相应的文本字符（如“k”或“M”）；在按下非字符键时， key 的值是相应键的名（如“Shift”或“Down”）。
  * **char**：在按下字符键时的行为与 key 相同，但在按下非字符键时值为 null。
  * **keyIdentifier**：在按下非字符键（例如 Shift）的情况下与 key 的值相同，返回一个格式类似“U+0000”的字符串，表示 Unicode 值
  * **location/keyLocation**（Safari 和 Chrome）：这是一个数值，表示按下了什么位置上的键：0 表示默认键盘，1 表示左侧位置（例如左位的 Alt 键），2 表示右侧位置（例如右侧的 Shift 键），3 表示数字小键盘，4 表示移动设备键盘（也就是虚拟键盘），5 表示手柄（如任天堂 Wii 控制器）。但即有 bug——值始终是 0，除非按下
  了数字键盘（此时，值 为 3）；否则，不会是 1、2、4、5

### 4.6文本事件

* **textInput**：在文本插入文本框之前会触发 textInput 事件。keypress->textInput，它俩区别之一就是任何可以获得焦点的元素都可以触发 keypress 事件，但只有可编辑区域才能触发 textInput事件。区别之二是 textInput 事件只会在用户按下能够输入实际字符的键时才会被触发，而 keypress事件则在按下那些能够影响文本显示的键时也会触发（例如退格键）。

  #### 文本事件特有的事件对象属性：

  * **data**：用户输入的字符（而非字符编码）。
  * **inputMethod**：只有IE支持，表示把文本输入到文本框中的方式。

### 4.7复合事件
IE9+唯一支持复合事件的浏览器。要确定浏览器是否支持复合事件，可以使用document.implementation.hasFeature("CompositionEvent", "3.0")
* **compositionstart**：在 IME 的文本复合系统打开时触发，表示要开始输入了。
* **compositionupdate**：在向输入字段中插入新字符时触发。
* **compositionend**：在 IME 的文本复合系统关闭时触发，表示返回正常键盘输入状态。
  #### 复合事件特有的事件对象属性：

  * **data**：如果在 compositionstart 事件发生时访问，包含正在编辑的文本（例如，已经选中的需要马上替换的文本）；如果在 compositionupdate 事件发生时访问，包含正插入的新字符；如果在 compositionend 事件发生时访问，包含此次输入会话中插入的所有字符
### 4.8变动事件
* **DOMSubtreeModified**：在 DOM 结构中发生任何变化时触发。这个事件在其他任何事件触发后都会触发。
* **DOMNodeInserted**：在一个节点作为子节点被插入到另一个节点中时触发。在这个事件触发时，节点已经被插入到了新的父节点中。**会冒泡**
* **DOMNodeRemoved**：在节点从其父节点中被移除时触发。在这个事件触发时，节点尚未从其父节点删除，因此其 parentNode 属性仍然指向父节点（与 event.relatedNode 相同）。**会冒泡**
* **DOMNodeInsertedIntoDocument**：会在新插入的节点上面触发。这个事件在 DOMNodeInserted 之后触发。**不会冒泡**
* **DOMNodeRemovedFromDocument**：如果被移除的节点包含子节点，那么在其所有子节点以及这个被移除的节点上会相继触发。这个事件在 DOMNodeRemoved 之后触发。**不会冒泡**
* **DOMAttrModified**：在特性被修改之后触发。
* **DOMCharacterDataModified**：在文本节点的值发生变化时触发。
### 4.9设备事件

* **orientationchange**：移动 Safari，只要用户改变了设备的查看（横向或纵向）模式就会触发。**window.orientation**，0 表示肖像模式，90 表示向左旋转的横向模式（“主屏幕”按钮在右侧），-90 表示向右旋转的横向模式（“主屏幕”按钮在左侧）
* MozOrientation：只有带加速计的设备才支持该事件，event 对象包含三个属性：x、y 和 z。这几个属性的值都介于 1 到-1 之间，表示不同坐标轴上的方向。在静止状态下，x 值为 0，y 值为 0，z 值为 1（表示设备处于竖直状态）。如果设备向右倾斜，x 值会减小；反之，向左倾斜，x 值会增大。类似地，如果设备向远离用户的方向倾斜，y 值会减小，向接近用户的方向倾斜，y 值会增大。z 轴检测垂直加速度度，1 表示静止不动，在设备移动时值会减小。（失重状态下值为 0）这是一个实验性 API，将来可能会变（可能会被其他事件取代）
* deviceorientation：与 MozOrientation 事件类似。它也是在加速计检测到设备方向变化时在 window 对象上触发，而且具有与 MozOrientation 事件相同的支持限制。不过，deviceorientation 事件的意图是告诉开发人员设备在空间中朝向哪儿，而
不是如何移动。事件对象包含以下 5 个属性。
    * alpha：在围绕 z 轴旋转时（即左右旋转时），y 轴的度数差；是一个介于 0 到 360 之间的浮点数。
    * beta：在围绕 x 轴旋转时（即前后旋转时），z 轴的度数差；是一个介于180 到 180 之间的浮点数。
    * gamma：在围绕 y 轴旋转时（即扭转设备时），z 轴的度数差；是一个介于90 到 90 之间的浮点数。
    * absolute：布尔值，表示设备是否返回一个绝对值。
    * compassCalibrated：布尔值，表示设备的指南针是否校准过。
* devicemotion：告诉开发人员设备什么时候移动，而不仅仅是设备方向如何改变。例如，通过 devicemotion 能够检测到设备是不是正在往下掉，或者是不是被走着的人拿在手里。事件对象包含以下属性：
    * acceleration：一个包含 x、y 和 z 属性的对象，在不考虑重力的情况下，告诉你在每个方向上的加速度。
    * accelerationIncludingGravity：一个包含 x、y 和 z 属性的对象，在考虑 z 轴自然重力加速度的情况下，告诉你在每个方向上的加速度。
    * interval：以毫秒表示的时间值，必须在另一个 devicemotion 事件触发前传入。这个值在每个事件中应该是一个常量。
    * rotationRate：一个包含表示方向的 alpha、beta 和 gamma 属性的对象。 

### 4.10触摸事件
下面这几个事件都会冒泡。
* **touchstart**：当手指触摸屏幕时触发；即使已经有一个手指放在了屏幕上也会触发。
* **touchmove**：当手指在屏幕上滑动时连续地触发。在这个事件发生期间，调用preventDefault()可以阻止滚动。
* **touchend**：当手指从屏幕上移开时触发。注意，在 touchend 事件发生时，touches集合中就没有任何 Touch 对象了，因为不存在活动的触摸操作；此时，就必须转而使用 changeTouchs集合。
* **touchcancel**：当系统停止跟踪触摸时触发。关于此事件的确切触发时间，文档中没有明确说明。
  #### 触摸事件特有的事件对象属性：
  每个触摸事件的event对象都提供了在鼠标事件中常见的属性：bubbles、cancelable、view、clientX、clientY、screenX、screenY、detail、altKey、shiftKey、ctrlKey 和 metaKey。还包含下列三个用于跟踪触摸的属性：

  * touches：表示当前跟踪的触摸操作的 Touch 对象的数组。
  * targetTouchs：特定于事件目标的 Touch 对象的数组。
  * changeTouches：表示自上次触摸以来发生了什么改变的 Touch 对象的数组。
  每个 Touch 对象包含下列属性：

      * clientX：触摸目标在视口中的 x 坐标。
      * clientY：触摸目标在视口中的 y 坐标。
      * identifier：标识触摸的唯一 ID。
      * pageX：触摸目标在页面中的 x 坐标。
      * pageY：触摸目标在页面中的 y 坐标。
      * screenX：触摸目标在屏幕中的 x 坐标。
      * screenY：触摸目标在屏幕中的 y 坐标。
      * target：触摸的 DOM 节点目标。

    
`补充：在触摸屏幕上的元素时，这些事件（包括鼠标事件）发生的顺序如下：(1) touchstart
(2) mouseover
(3) mousemove（一次）
(4) mousedown
(5) mouseup
(6) click
(7) touchend 目前只有 iOS 版 Safari 支持多点触摸。
桌面版 Firefox 6+和 Chrome 也支持触摸事件`
### 4.11手势事件
只有两个手指都触摸到事件的接收容器时才会触发这些事件。这些事件都冒泡。当一个手指放在屏幕上时，会触发 touchstart 事件。如果另一个手指又放在了屏幕上，则会先触发 gesturestart 事件，随后触发基于该手指的 touchstart事件。如果一个或两个手指在屏幕上滑动，将会触发 gesturechange 事件。但只要有一个手指移开，就会触发 gestureend 事件，紧接着又会触发基于该手指的 touchend 事件。
* **gesturestart**：当一个手指已经按在屏幕上而另一个手指又触摸屏幕时触发。
* **gesturechange**：当触摸屏幕的任何一个手指的位置发生变化时触发。
* **gestureend**：当任何一个手指从屏幕上面移开时触发。
  #### 手势事件特有的事件对象属性：
  与触摸事件一样，bubbles、cancelable、view、clientX、clientY、screenX、screenY、detail、altKey、shiftKey、ctrlKey 和 metaKey。还包含两个额外的属性：

  * **rotation**：表示手指变化引起的旋转角度，负值表示逆时针旋转，正值表示顺时针旋转（该值从 0 开始）。
  * **scale**：表示两个手指间距离的变化情况（例如向内收缩会缩短距离）；这个值从 1 开始，并随距离拉大而增长，随距离缩短而减小。
  
### 4.12表单事件
除了支持鼠标、键盘、更改和 HTML 事件之外，所有表单字段都支持下列前 3 个事件：
* **blur**：当前字段失去焦点时触发。
* **focus**：当前字段获得焦点时触发。
* **change**：对于input和textarea元素，在它们失去焦点且 value 值改变时触发；对于select元素，在其选项改变时触发。
* **select**：与 select()方法对应的，是一个 select 事件。在选择了文本框中的文本时，就会触发 select事件。在 IE9+、Opera、Firefox、Chrome
和 Safari 中，只有用户选择了文本（而且要释放鼠标），才会触发 select 事件。而在 IE8 及更早版本中，只要用户选择了一个字母（不必释放鼠标），就会触发 select 事件。提供俩个属性：selectionStart 和 selectionEnd（选择文本的偏移量）。但是ie9之前不支持，可以通过document.selection 对象，其中保存着用户在整个文档范围内选择的文本信息。如下代码块兼容写法：
`input（size属性表示显示的字符数）和textarea（rows、cols文本框的字符行列数）文本框都支持 select()和setSelectionRange()方法，select()这个方法用于选择文本框中的所有文本。setSelectionRange(要选择的第一个字符的索引,要选择的最后一个字符的索引)选择部分文本，但是ie<9以下要通过 createTextRange()先在文本框上创建一个范围，在设置collapse()将范围折叠到文本框的开始位置，再使用 moveStart()和 moveEnd()这两个范围方法将范围移动到位，最后一步，就是使用范围的 select()方法选择文本`
* **beforecopy**：在发生复制操作前触发。
* **copy**：在发生复制操作时触发。
* **beforecut**：在发生剪切操作前触发。
* **cut**：在发生剪切操作时触发。
* **beforepaste**：在发生粘贴操作前触发。
* **paste**：在发生粘贴操作时触发。取消默认事件就不会粘贴了，上面剪切事件也是如此。以上6个为剪切事件，特有的事件对象的属性如下：
   * **clipboardData**：有三个方法：getData()、setData()和 clearData()。其中，getData()用于从剪贴板中取得数据，它接受一个参数，即要取得的数据的格式。在 IE 中，有两种数据格式："text"和"URL"。在 Firefox、Safari 和 Chrome 中，这个参数是一种 MIME 类型；不过，可以用"text"代表
"text/plain"。类似地，setData()方法的第一个参数也是数据类型，第二个参数是要放在剪贴板中的文本。但是，与getData()方法不同的是，Safari 和 Chrome 的 setData()方法不能识别"text"类型。这两个浏览器在成功将文本放到剪贴板中后，都会返回 true；否则，返回 false。
```
//跨浏览器取得选择的文本
function getSelectedText(textbox){
 if (typeof textbox.selectionStart == "number"){
   return textbox.value.substring(textbox.selectionStart,textbox.selectionEnd);
 } else if (document.selection){
   return document.selection.createRange().text;
 }
} 
//跨浏览器选择部分文本
function selectText(textbox, startIndex, stopIndex){
  if (textbox.setSelectionRange){
    textbox.setSelectionRange(startIndex, stopIndex);
  } else if (textbox.createTextRange){
    var range = textbox.createTextRange();
    range.collapse(true);
    range.moveStart("character", startIndex);
    range.moveEnd("character", stopIndex - startIndex);
    range.select();
  } 
  textbox.focus();
}
```
### 4.13拖放事件
默认情况下，图像、链接和文本是可以拖动的，对于其他元素可以设置draggable 属性。在拖动元素经过某些无效放置目标时，可以看到一种特殊的光标（圆环中有一条反斜线），表示不能放置。虽然所有元素都支持放置目标事件，但这些元素默认是不允许放置的。如果拖动元素经过不允许放置的元素，无论用户如何操作，都不会发生 drop 事件。不过，你可以把任何元素变成有效的放置目标，方法是组织 dragenter 和 dragover 事件的默认行为。这样当拖动着元素移动到放置目标上时，光标变成了允许放置的符号。当然，释放鼠标也会触发 drop 事件。在 Firefox 3.5+中，放置事件的默认行为是打开被放到放置目标上的 URL。换句话说，如果是把图像拖放到放置目标上，页面就会转向图像文件；而如果是把文本拖放到放置目标上，则会导致无效 URL错误。因此，为了让 Firefox 支持正常的拖放，还要取消 drop 事件的默认行为
* **dragstart**：按下鼠标键并开始移动鼠标时，会在被拖放的元素上触发 dragstart 事件。
* **drag**：触发 dragstart 事件后，随即会触发 drag 事件，而且在元素被拖动期间会持续触发该事件。这个事件与 mousemove 事件相似，在鼠标移动过程中，mousemove 事件也会持续发生。
* **dragend**：当拖动停止时（无论是把元素放到了有效的放置目标，还是放到了无效的放置目标上），会触发 dragend 事件。
* **dragenter**：只要有元素被拖动到放置目标上，就会触发 dragenter 事件（类似于 mouseover 事件）。
* **dragover**：紧随其后的是 dragover 事件，而且在被拖动的元素还在放置目标的范围内移动时，就会持续触发该事件。
* **dragleave**：如果元素被拖出了放置目标，dragover 事件不再发生，但会触发 dragleave 事件（类似于 mouseout事件）。
* **drop**：如果元素被放到了放置目标中，则会触发 drop 事件。特有的事件对象的属性如下：
   * **dataTransfer**：两个主要方法：getData()和 setData()。getData()可以取得由 setData()保存的值。他俩的第一个参数为MIME类型，考虑到向后兼容，HTML5 也支持"text"和"URL"，但这两种类型会被映射为"text/plain"和"text/uri-list"，保存在 dataTransfer 对象中的数据只能在 drop事件处理程序中读取。如果在 ondrop 处理程序中没有读到数据，那就是 dataTransfer 对象已经被销毁，数据也丢失了。将数据保存为文本和保存为 URL 是有区别的。如果将数据保存为文本格式，那么数据不会得到任何特殊处理。而如果将数据保存为 URL，浏览器会将其当成网页中的链接。换句话说，如果你把它放置到另一个浏览器窗口中，浏览器就会打开该 URL。
      * dropEffect：可以知道被拖动的元素能够执行哪种放置行为。必须在 ondragenter 事件处理程序中针对放置目标来设置它。"none"：不能把拖动的元素放在这里。这是除文本框之外所有元素的默认值。"move"：应该把拖动的元素移动到放置目标。"copy"：应该把拖动的元素复制到放置目标。"link"：表示放置目标会打开拖动的元素（但拖动的元素必须是一个链接，有 URL）。dropEffect 属性只有搭配 effectAllowed 属性才有用。
      * effectAllowed：表示允许拖动元素的哪种 dropEffect。"uninitialized"：没有给被拖动的元素设置任何放置行为。"none"：被拖动的元素不能有任何行为。"copy"：只允许值为"copy"的 dropEffect。"link"：只允许值为"link"的 dropEffect。 "move"：只允许值为"move"的 dropEffect。"copyLink"：允许值为"copy"和"link"的 dropEffect。"copyMove"：允许值为"copy"和"move"的 dropEffect。"linkMove"：允许值为"link"和"move"的 dropEffect。"all"：允许任意 dropEffect。必须在 ondragstart 事件处理程序中设置 effectAllowed 属性。
      * addElement(element)：为拖动操作添加一个元素。添加这个元素只影响数据（即增加作为拖动源而响应回调的对象），不会影响拖动操作时页面元素的外观。
      * clearData(format)：清除以特定格式保存的数据。
      * setDragImage(element, x, y)：指定一幅图像，当拖动发生时，显示在光标下方。这个方法接收的三个参数分别是要显示的 HTML 元素和光标在图像中的 x、y 坐标。其中，HTML 元素可以是一幅图像，也可以是其他元素。是图像则显示图像，是其他元素则显示渲染后的元素。
      * types：当前保存的数据类型。这是一个类似数组的集合，以"text"这样的字符串形式保存着数据类型。
## 5内存和性能
在 JavaScript 中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。导致这一问题的原因是多方面的。首先，每个函数都是对象，都会占用内存；内存中的对象越多，性能就越差。其次，必须事先指定所有事件处理程序而导致的 DOM 访问次数，会延迟整个页面的交互就绪时间。方案一：对“事件处理程序过多”问题的解决方案就是**事件委托**。所有用到按钮的事件（click、mousedown、mouseup、keydown、keyup 和 keypress）都适合采用事件委托技术。虽然 mouseover 和 mouseout 事件也冒泡，但要适当处理它们并不容易，而且经常需要计算元素的位置。（因为当鼠标从一个元素移到其子节点时，或者当鼠标移出该元素时，都会触发 mouseout 事件。）如果可行的话，也可以考虑为 document 对象添加一个事件处理程序。这样好处：

1. document 对象很快就可以访问，而且可以在页面生命周期的任何时点上为它添加事件处理程序（无需等待 DOMContentLoaded 或 load 事件）。换句话说，只要可单击的元素呈现在页面上，就可以立即具备适当的功能。
2. 在页面中设置事件处理程序所需的时间更少。只添加一个事件处理程序所需的 DOM 引用更少，所花的时间也更少。
3. 整个页面占用的内存空间更少，能够提升整体性能。
方案二：移除事件处理程序，你知道某个元素即将被移除，那么最好手工移除事件处理程序，有的浏览器（尤其是 IE）在这种情况下不会作出恰当地处理，它们很有可能会将对元素和对事件处理程序的引用都保存在内存中；在页面卸载之前，先通过 onunload 事件处理程序移除所有事件处理程序
## 6模拟事件

通过JavaScript 在任意时刻来触发特定的事件，DOM2 级规范为此规定了模拟特定事件的方式，IE9、Opera、Firefox、Chrome 和 Safari 都支持这种方式。IE 有它自己模拟
事件的方式。
### 6.1DOM中的事件模拟
通过document.createEvent()来创建 event 对象，这个方法接收一个参数（若为CustomEvent，自定义事件，返回的对象有一个名为initCustomEvent(type,bubbles（布尔值,是否应该冒泡）,cancelable（布尔值,是否可以取消）,detail（任意值，保存在 event 对象的 detail 属性中的方法）)，即表示要创建的事件类型的字符串。并通过调用 dispatchEvent()来触发事件，，需要传入一个参数，即表示要触发事件的 event 对象。在 DOM2 级中，所有这些字符串都使用英文复数形式，而在 DOM3级中都变成了单数。这个字符串可以是下列几字符串之一。

* UIEvents：一般化的 UI 事件。鼠标事件和键盘事件都继承自 UI 事件。DOM3 级中是 UIEvent。
* MouseEvents：一般化的鼠标事件。DOM3 级中是 MouseEvent。模拟鼠标事件：initMouseEvent()来创建事件对象。模拟键盘事件：initKeyEvent()
* MutationEvents：一般化的 DOM 变动事件。DOM3 级中是 MutationEvent。initMutationEvent()
* HTMLEvents：一般化的 HTML 事件。没有对应的 DOM3 级事件（HTML 事件被分散到其他类别中）。
### 6.2IE中的事件模拟
在 IE8 及之前版本中模拟事件与在 DOM 中模拟事件的思路相似：先创建 event 对象，然后为其指定相应的信息，然后再使用该对象来触发事件。document.createEventObject()不接受参数，会返回一个通用的 event 对象，然后，你必须手工为这个对象添加所有必要的信息（eg：event.screenX = 100;），是在目标上调用 fireEvent(事件处理程序名称（eg："onkeypress"）, event)方法，会自动为event 对象添加 srcElement 和 type 属性；其他属性则都是必须通过手工添加的。

```
//跨浏览器封装
var EventUtil = {
 //绑定事件
 addHandler: function(element, type, handler){
     if (element.addEventListener){
        element.addEventListener(type, handler, false);
     } else if (element.attachEvent){
        element.attachEvent("on" + type, handler);
     } else {
        element["on" + type] = handler;
     }
 },
 //移除事件
 removeHandler: function(element, type, handler){
     if (element.removeEventListener){
        element.removeEventListener(type, handler, false);
     } else if (element.detachEvent){
        element.detachEvent("on" + type, handler);
     } else {
        element["on" + type] = null;
     }
 },
 //事件对象
 getEvent: function(event){
    return event ? event : window.event;
 },
 //事件目标
 getTarget: function(event){
    return event.target || event.srcElement;
 },
 //阻止默认行为
 preventDefault: function(event){
     if (event.preventDefault){
        event.preventDefault();
     } else {
        event.returnValue = false;
     }
 }, 
 //阻止冒泡
 stopPropagation: function(event){ 
     if (event.stopPropagation){
        event.stopPropagation();
     } else {
        event.cancelBubble = true;
     }
 },
 //获取相关元素
 getRelatedTarget: function(event){
     if (event.relatedTarget){
        return event.relatedTarget;
     } else if (event.toElement){
        return event.toElement;
     } else if (event.fromElement){
        return event.fromElement;
     } else {
        return null;
     }
 }, 
 //按钮类型 IE8 及之前版本也提供了 button 属性，但这个属性的值与DOM的button 属性有很大差异。所以做了以下匹配：
 getButton: function(event){
     if (document.implementation.hasFeature("MouseEvents", "2.0")){
        return event.button;
     } else {
         switch(event.button){
             case 0: //表示没有按下按钮
             case 1: //表示按下了主鼠标按钮
             case 3: //表示同时按下了主、次鼠标按钮
             case 5: //表示同时按下了主鼠标按钮和中间的鼠标按钮
             case 7: //表示同时按下了三个鼠标按钮
                return 0; 
             case 2: //表示按下了次鼠标按钮
             case 6: //表示同时按下了次鼠标按钮和中间的鼠标按钮
                return 2;
             case 4: //表示按下了中间的鼠标按钮
                return 1;
         }
     }
 }, 
 getWheelDelta: function(event){
     if (event.wheelDelta){
     //当用户向前滚动鼠标滚轮时，wheelDelta 是 120 的倍数；当用户向后滚动鼠标滚轮时，wheelDelta 是-120 的倍数；但是，在 Opera9.5之前的版本中，wheelDelta 值的正负号是颠倒的
        return (client.engine.opera && client.engine.opera < 9.5 ?
        -event.wheelDelta : event.wheelDelta);
     } else {
     //Firefox 当向前滚动鼠标滚轮时，这个属性的值是-3 的倍数，当向后滚动鼠标滚轮时，这个属性的值是 3 的倍数
        return -event.detail * 40;
     } 
 },
 getCharCode: function(event){
     if (typeof event.charCode == "number"){
     //在不支持charCode这个属性的浏览器中，值为 undefined
        return event.charCode;
     } else {
     //IE8及之前版本和Opera则是在keyCode 中保存字符的 ASCII编码
        return event.keyCode;
     }
 },
 //获取剪切板数据
 getClipboardText: function(event){
    var clipboardData = (event.clipboardData || window.clipboardData);
    return clipboardData.getData("text");
 }, 
 //设置剪切板数据
 setClipboardText: function(event, value){
    if (event.clipboardData){
      return event.clipboardData.setData("text/plain", value); 
    } else if (window.clipboardData){
      return window.clipboardData.setData("text", value);
    }
 }, 
}; 
功能函数：
//屏蔽非数值字符但不屏蔽那些也会触发 keypress 事件的基本按键
EventUtil.addHandler(textbox, "keypress", function(event){
   event = EventUtil.getEvent(event);
   var target = EventUtil.getTarget(event);
   var charCode = EventUtil.getCharCode(event);
   if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9  &&
      !event.ctrlKey){ //在除 IE 之外的所有浏览器中，前面的代码也会屏蔽 Ctrl+C、Ctrl+V，以及其他使用 Ctrl 的组合键。因此，最后还要添加一个检测条件，以确保用户没有按下 Ctrl 键.虽然理论上只应该在用户按下字符键时才触发 keypress 事件，但有些浏览器也会对其他键触发此事件。Firefox 和 Safari（3.1 版本以前）会对向上键、向下键、退格键和删除键触发 keypress 事件；Safari 3.1 及更新版本则不会对这些键触发 keypress 事件。这意味着，仅考虑到屏蔽不是数值的字符还不够，还要避免屏蔽这些极为常用和必要的键。所幸的是，要检测这些键并不困难。在 Firefox 中，所有由非字符键触发的 keypress 事件对应的字符编码为 0，而在 Safari 3 以前的版本中，对应的字符编码全部为 8。为了让代码更通用，只要不屏蔽那些字符编码小于 10 的键即可。
      EventUtil.preventDefault(event);//屏蔽按键
   }
}); 
//自动切换焦点
function tabForward(event){
   event = EventUtil.getEvent(event);
   var target = EventUtil.getTarget(event);
   if (target.value.length == target.maxLength){
      var form = target.form;
      for (var i=0, len=form.elements.length; i < len; i++) {
         if (form.elements[i] == target) {
            if (form.elements[i+1]){
               form.elements[i+1].focus();
            }
            return;
         }
      }
   }
 } 
```
