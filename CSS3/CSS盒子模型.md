#CSS盒子模型
##盒子模型简介
<img src="img/屏幕截图%202022-07-09%20165505.jpg">

>元素框的最内部分是实际的内容，直接包围内容的是内边距。内边距呈现了元素的背景。内边距的边缘是边框。边框以外是外边距，外边距默认是透明的，因此不会遮挡其后的任何元素。内边距、边框和外边距都是可选的，默认值是零，但是，许多元素将由用户代理样式表设置外边距和内边距。可以通过将元素的 **margin** 和 **padding** 设置为零来覆盖这些浏览器样式。

---
##边框
###边框样式
***border-style*** 属性指定边框样式，分别可以取以下的值
* **solid** 实现框
* **dotted** 点状框
* **dashed** 虚线框
* **double** 双线框
###边框宽度
***border-width*** 用于指定边框宽度，可以设置1到4个值
~~~CSS
div.one {
  border-style: solid;
  border-width: 10px; /* 所有边框设置为10px */
}

div.two {
  border-style: solid;
  border-width: 20px 5px; /* 上下边框为 20px，左右边框为 5px */
}
/*顺时针，上右下左*/
div.three {
  border-style: solid;
  border-width: 25px 10px 4px 35px; /* 上边框 25px，右边框 10px，下边框 4px，左边框 35px */
}
~~~
###边框颜色
***border-color*** 用于指定边框颜色，
###简写边框属性
为了缩减代码，也可以在一个属性中指定所有单独的边框属性。border 属性是以下各个边框属性的简写属性：
* **border-width**
* **border-style（必需）**
* **border-color**
###边框各边
如果只想指定某一个边框具有特定的属性，可以这样做。
~~~CSS
div{
    border-top:5px solid red
}
~~~
###圆角边框
**border-radius** 属性用于向元素添加圆角边框，当边框为正方形时，设置圆角边框的大小为边长的一半就可以使得边框变成圆形
~~~CSS
.circle{
  width:100px;
  height:100px;
  border-radius:50px;
  background-color:red;
}
~~~

---
##外边距
###简写属性
简写属性同样按照上右下左指定四个边框外边距。
###auto值
通过设置外边距外aoto可以实现元素在其容器中**水平居中**，该元素将占据指定的宽度，左右外边距平均分配。
~~~CSS
div {
  width: 300px;
  margin: auto;
  border: 1px solid red;
}
~~~
###外边距为负值
外边距是允许为负值的，当设置为负值时，元素内容将越过边框。通常这样做是很有用的，**因为暂时没有遇到比较好的例子就不举例说明，之后遇到了再做补充**
###外边距合并
>外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

详细解释和案例可以看<a href="https://www.w3school.com.cn/css/css_margin_collapse.asp">外边距合并</a>

---
##内边距
内边距的使用和外边距基本相似，在实际应用中往往会碰到这样一类问题：
>我们指定了内容的宽度，在添加内边距时又会使元素宽度增加，而实际上我们想要保持元素宽度的大小不变。

解决这个问题可以使用**box-sizing**属性，它将固定元素的宽度，当增加内边距时，内容的宽度相应变小。
~~~CSS
div {
  width: 300px;
  padding: 25px;
  box-sizing: border-box;
}
~~~
**Tips**:可以对所有元素应用 **box-sizing**，并且这样的做法时安全且明智的。
