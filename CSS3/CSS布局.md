#CSS布局
个人感觉CSS最难的部分是布局和定位这一块，每次想要调整一个区域到自己想要的哪个地方得花好多时间，而且布局这一部分有很多知识点，比较分散，而且一种布局有很多实现方式，但是具体应用场景有些方法又不适用，因此有必要做一个比较详细的总结。
##display属性
>display 属性规定是否/如何显示元素。每个 HTML 元素都有一个默认的 display 值，具体取决于它的元素类型。大多数元素的默认 display 值为 block 或 inline。
###block
块级元素总是从新行开始，并占据可用的全部宽度。块级元素的一些例子
* p
* div
* h1~h6
* section
* header
* footer
###inline
内联元素不从新行开始，仅占用所需的宽度。行内元素的一些例子
* span
* a
* img
###覆盖默认样式
虽然每个元素有默认的display样式，但是可以覆盖它们，一个常见的例子是使用 ***li*** 元素生成水平的菜单栏。
~~~CSS
li{
    display:block;
}
~~~
##定位
##浮动
##FlexBox
###Flex容器定义
当把父元素设置**display**属性设置为**flex**，父元素成为一个flex容器，子元素成为flex项目，可以自由的伸缩大小
###flex-direction
flex-direction 属性定义容器要在哪个方向上堆叠 flex 项目。
~~~CSS
/* 垂直方式堆叠项目 */
.flex-container {
  display: flex;
  flex-direction: column;
}
/* 水平方式堆叠项目 */
.flex-container {
  display: flex;
  flex-direction: row;
}
~~~
###flex-wrap
flex-wrap 属性规定是否应该对 flex 项目换行
~~~CSS

~~~
