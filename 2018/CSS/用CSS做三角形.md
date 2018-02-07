# CSS做三角形

主要用到的属性是` border `

先看一段代码：
``` css
.border_tr{
  width: 0;
  height: 0;
  border-width: 15px;
  border-style: solid;
  border-color: #aaa #bbb #ccc #ddd;
}
```

这段代码可以实现将一个宽高为15px的正方形，切割为以正方形中点为顶点的四个等边三角形。


不等边三角形，就是把` border-width `设置为长方形。
``` css
border-width: 15px 25px 0px 0px;
```
> 注意：这里设置的数值不能为左右和上下。这样无法实现三角形。


如果把上述代码稍加改变，可以实现任意三角形。方法是将三角形分割为两个甚至多个三角形。


其实这里实现的原理是：

1. 将` div `设置为宽高都为0的元素，用border填充div。
2. 设置` border-width `来实现三角形的形状以及宽高。
3. 设置` border-color `来实现想要的颜色。
