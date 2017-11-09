### DOM

DOM是JavaScript操作网页的接口，全称“文档对象摸象”（Document Object Model）。它的作用就是将网页转化为一个JavaScript对象，从而可以通过脚本进行各项操作。

浏览器根据DOM，将结构化文档（比如HTML和XML）解析称一系列节点，再有这些节点组成树状结构（DOM Tree）。所有的节点和最终的树状结构都有规范的对外接口。

DOM不属于JavaScript，但操作DOM是JavaScript最常见的任务，而JavaScript也是操作DOM的常用语言。

#### DOM节点

* `Document`：整个文档树的顶层节点
* `DocumentType`：doctype标签（比如<!DOCTYPE html>）
* `Element`：网页的各种HTML标签（比如 &lt;body&gt; 、&lt;a&gt;等）
* `Attribute`：网页元素的属性（比如class="right"）
* `Text`：标签之间或标签包含的文本
* `Comment`：注释
* `DocumentFragment`：文档的片段

#### 节点树
一个文档的所有节点，按照所在的层级，可以抽象成一种树状结构。这种树状结构就是DOM。

顶层的节点就是document节点，它代表了整个文档。文档里面最高一层的HTML标签，一般是&lt;html&gt；，它构成树结构的根节点（root node），其他HTML标签节点都是它的下级。

节点之间的关系

* 父节点关系（`parentNode`）：直接的那个上级节点
* 子节点关系（`childNodes`）：直接的下级节点
* 同级节点关系（`sibling`）：拥有同一个父节点的节点

#### 特征相关的属性
所有节点对象都是浏览器内置的Node对象的实例，继承了Node属性和方法。这是所有节点的共同特征。

* Node.nodeName，Node.nodeType
* Node.nodeValue
* Node.textContent
* Node.baseURI

#### 相关节点属性

* Node.ownerDocument
* Node.nextSibling
* Node.previousSibling
* Node.parentNode
* Node.parentElement
* Node.childNodes
* Node.firstChild，Node.lastChild

#### 节点对象的方法

* Node.appendChild()
* Node.hasChildNodes()
* Node.cloneNode()
* Node.insertBefore()
* Node.removeChild()
* Node.replaceChild()
* Node.contains()
* Node.compareDocumentPosition()
* Node.isEqualNode()
* Node.normalize()

#### NodeList，HTMLCollection
document.querySelectorAll() 返回的就是一个NodeList，它一个类数组的对象

document.images 返回的是一个HTMLCollection， 它也是一个类数组对象

HTMLCollection与NodeList的区别有以下几点。

（1）HTMLCollection实例对象的成员只能是Element节点，NodeList实例对象的成员可以包含其他节点。

（2）HTMLCollection实例对象都是动态集合，节点的变化会实时反映在集合中。NodeList实例对象可以是静态集合。

（3）HTMLCollection实例对象可以用id属性或name属性引用节点元素，NodeList只能使用数字索引引用。

#### ParentNode接口，ChildNode接口

`ParentNode`接口用于获取Element子节点。Element节点、Document节点和DocumentFragment节点，部署了ParentNode接口。凡是这三类节点，都具有以下四个属性，用于获取Element子节点。

（1）**children**
返回一个动态的HTMLCollection集合，由当前节点的所有Element子节点组成。

（2）**firstElementChild**
反回当前节点的第一个Element子节点，如果不存在任何Element子节点，则返回null。

（3）**lastElementChild**
返回当前节点的最后一个Element子节点，如果不存在任何Element子节点，则返回null。

（4）**childElementCount**
返回当前节点的所有Element子节点的数目。

`ChildNode`接口用于处理子节点（包含但不限于Element子节点）。Element节点、DocumentType节点和CharacterData接口，部署了ChildNode接口。凡是这三类节点（接口），都可以使用下面四个方法。

（1）**remove()**
用于移除当前节点。

（2）**before()**
用于在当前节点的后面，插入一个同级节点。如果参数是节点对象，插入DOM的就是该节点对象；如果参数是文本，插入DOM的就是参数对应的文本节点。

（3）**after()**
用于在当前节点的后面，插入一个同级节点。如果参数是节点对象，插入DOM的就是该节点对象；如果参数是文本，插入DOM的就是参数对应的文本节点。

（4）**replaceWith()**
使用参数指定的节点，替换当前节点。如果参数是节点对象，替换当前节点的就是该节点对象；如果参数是文本，替换当前节点的就是参数对应的文本节点。

###  BOM

在浏览器中，window对象指当前的浏览器窗口。它也是所有对象的顶层对象。

#### window对象的属性

* window.window，window.name
* window.location
* window.closed，window.opener
* window.screenX，window.screenY
* window.innerHeight，window.innerWidth
* window.outerHeight，window.outerWidth
* window.pageXOffset，window.pageYOffset

#### navigator对象

* navigator.userAgent
* navigator.platform
* navigator.onLine
* **navigator.geolocation** 返回用户地理位置的信息

#### history

* history.back()：移动到上一个访问页面，等同于浏览器的后退键。
* history.forward()：移动到下一个访问页面，等同于浏览器的前进键。
* history.go()：接受一个整数作为参数，移动到该整数指定的页面，比如go(1)相当于forward()，go(-1)相当于back()。
* history.pushState()和history.replaceState()，用来在浏览历史中添加和修改记录。
* history.state：返回当前页面的state对象。

#### window.screen对象

* screen.height和screen.width

#### window对象的方法

* window.moveTo()，window.moveBy()
* window.scrollTo()，window.scrollBy()
* window.open(), window.close()
* window.print() 打印
* window.focus() 激活指定窗口
* window.getSelection() 用户现在选中的文本

#### 窗口的引用

* top：顶层窗口，即最上层的那个窗口
* parent：父窗口
* self：当前窗口，即自身

#### 事件

* load事件和onload属性
* error事件和onerror属性
* popstate配合history对象使用

#### URL的编码/解码方法
* encodeURI()
* encodeURIComponent()
* decodeURI()
* decodeURIComponent()

#### alert()，prompt()，confirm()




