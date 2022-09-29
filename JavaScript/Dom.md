#<span id="Top">DOM</span>

>文档对象模型 ***（DOM，Document Object Mdodel）*** 是一个应用编程接口，DOM将整个页面抽象为一组分层节点。HTML页面的每个部分都是一个结点。DOM通过创建表示文档的树，能够让开发者随心所欲的控制网页的内容和结构，使用DOMAPI可以轻松的删除和修改节点。

本文主要介绍 ***DOM*** 的两个知识点：
DOM的的<u>[基本操作](#DOM-1)</u>和<u>[DOM事件](#DOM-2)</u>

---

##<span id="DOM-1">DOM的基本操作</span>
* [查](#DOM-1-1)
* [改](#DOM-1-2)
* [增](#DOM-1-3)
* [删](#Dom-1-4)

###<span id="DOM-1-1">查找DOM节点</span>
获取 **DOM** 节点有以下几种方式
1. 根据document的成员函数，通过id,tagname,class来获取DOM节点
2. 通过CSS选择器来获取
3. 通过已获得的节点对象，来来访 **DOM** 对象的兄弟或子节点

####Document
~~~javascript{.line-numbers}
    //通过ID
    let domresult = document.getElementByID("mytitle");
    //通过tagname
    let domresult = document.getElementsBytagName("li");
    //通过class
    let domresult = document.getElementsByClassName("detail");
    //后两种方式返回的是一个数组的nodelist
~~~
####CSS选择器
~~~javascript{.line-numbers}
    //匹配第一个标签为p的节点
    let domresult = document.querySelector("p");
    //匹配所有标签为p的节点
    let domresult = document.querytSelectorAll("p");
    //匹配class属性为detail的节点
    let domresult = document.querySelector(".detail");
    //匹配id为right的几点
    let domresult = document.querySelector("#right")
    //匹配ul的所有li
    let domresult = document.querySelectorAll("ul > li");
    //匹配main下的所有子节点
    let domresult = document.querySelectorAll("main > *");
~~~
####通过当前节点
* ***.children*** 获取子节点
* ***.parentNode*** 获取父节点
* ***.nextElementSibling*** 紧跟在后面的兄弟节点

###<span id="DOM-1-2">修改DOM节点</span>
~~~javascript{.line-numbers}
    let titleDom = getElementByID("novel");
    //通过innerHTML，需要插入HTML标签
    titleDom.innerHTML = "<strong>神雕侠侣</strong>";
    //通过textContent，
    titleDom.textContent = "射雕英雄传";
    //修改style属性值
    titleDom.style = "color: blue"
    //通过setAttribute

~~~
###<span id="DOM-1-3">增加DOM节点</span>
~~~javascript{.line-numbers}
//如下是一个示例：把时间插入到页面中

~~~
###<span id="DOM-1-4">删除DOM节点</span>

---

##<span id="DOM-2">DOM事件</span>
DOM事件是 **JavaScrip**与 **HTML**交互的基础
常见事件类别有：
1.鼠标事件

监听事件的方式有两种
* **DOM** 0级：通过给属性值赋值一个函数
* **DMO** 2级：使用事件监听函数 ***addEventListener()***

### DOM 0级
~~~javascript{.line-numbers}
//onclick事件

//onload事件

//onchange事件

//onmouse和onmouseout事件
~~~
**DOM 0级**只能绑定一个回调函数
### DOM 2级
***addEventListener()*** 函数有三个参数，第一个是事件类型，第二个回调函数，第三个为布尔值，指定使用事件冒泡还是事件捕获。与0级不同的是，可以为元素指定多个事件处理程序
~~~javascript{.line-numbers}
//添加单个处理程序
element.addEventListener("click", function(){ alert("Hello World!"); });
//添加多个程序
element.addEventListener("mouseover", myFunction);
element.addEventListener("click", mySecondFunction);
element.addEventListener("mouseout", myThirdFunction);
//向Window对象添加处理程序
window.addEventListener("resize", function(){
  document.getElementById("demo").innerHTML = Math.random();
~~~
**疑惑：DOM事件是嵌套的的关系，如果给ul和body绑定一个事件，当点击li时，他们是否会触发事件？**
先介绍DOM事件流：事件流分为三个阶段：**事件捕获阶段**-->**目标事件阶段**-->**事件冒泡阶段**。
当触发目标事件时，会先进入目标捕获阶段，会从HTML的顶部一次向下传播到达目标事件，然后开始执行回调函数，执行结束后，事件冒泡，从目标事件再传播到顶部。因此会先出发li绑定的事件，然后 ***ul***，最后到 ***body***。



**持续完善中~~** <u>[回到顶部](#Top)</u>