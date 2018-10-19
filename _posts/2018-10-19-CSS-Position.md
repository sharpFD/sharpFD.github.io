---
layout:     post
title:      "CSS中position属性( absolute | relative | static | fixed )详解"
subtitle:   "相对定位？绝对定位？ 我来告诉你这是什么"
date:       2018-10-19
author:     "吴云根"
header-img: "img/2018_10_19/position.png"
tags:
    - CSS
---


## 目录
* [Position的定义](#定义)
* [什么是文档流](#什么是文档流)
* [几种定位方式](#几种定位方式)
    * [静态定位(static)](#静态定位(static))
    * [相对定位(relative)](#相对定位(relative))
    * [绝对定位(absoulte)](#绝对定位(absoulte))
    * [固定定位(fixed)](#固定定位(fixed))
* [Z-index属性](#Z-index属性)
    * xxx

### 定义
    static：无特殊定位，对象遵循正常文档流。top，right，bottom，left等属性不会被应用。  
    relative：对象遵循正常文档流，但将依据top，right，bottom，left等属性在正常文档流中偏移位置。而其层叠通过z-index属性定义。
    absolute：对象脱离正常文档流，使用top，right，bottom，left等属性进行绝对定位。而其层叠通过z-index属性定义。
    fixed：对象脱离正常文档流，使用top，right，bottom，left等属性以窗口为参考点进行定位，当出现滚动条时，对象不会随着滚动。而其层叠通过z-index属性定义。
    
怎么样，是不是还是很迷糊~~ 没关系，下面就从几个基础概念一一给大家详述：

---

### 什么是文档流
* 将窗体自上而下分成一行行, 并在每行中按从左至右的顺序排放元素,即为文档流。
* 只有三种情况会使得元素脱离文档流，分别是：**浮动**、**绝对定位**和**相对定位**。

---

### 几种定位方式
* #### 静态定位(static) 
```
static，无特殊定位，它是html元素默认的定位方式，即我们不设定元素的position属性时默认的position值就是static，它遵循正常的文档流对象，对象占用文档空间，  
该定位方式下，top、right、bottom、left、z-index等属性是无效的。
```

* #### 相对定位(relative) 
```
relative定位，又称为相对定位，从字面上来解析，我们就可以看出该属性的主要特性：相对。但是它相对的又是相对于什么地方而言的呢？这个是个重点，也是最让我迷糊的一个  
地方，现在让我们来做个测试，我想大家都会明白的：
```
(1) 初始未定位
   
```html
/******初始*********/
<style type="text/css">
    #first { width: 200px; height: 100px; border: 1px solid red; }
    #second{ width: 200px; height: 100px; border: 1px solid blue;}
</style>
<body>
   <div id="first"> first</div>
   <div id="second">second</div>
</body>
```

###### 初始原图：

![loading](/img/2018_10_19/no-position.png "初始原图")

---

(2) 我们修改first元素的position属性：
```html
<style type="text/css">
    #first{ width: 200px; height: 100px; border: 1px solid red; position: relative; top: 20px; left: 20px;} /*add position*/
    #second{width: 200px; height: 100px; border: 1px solid blue;}
</style>
```
######  相对偏移20px后：

![loading](/img/2018_10_19/position20.png "相对偏移20px后")

 > 现在看明白了吧，`相对定位相对的是它原本在文档流中的位置而进行的偏移`，而我们也知道relative定位也是遵循正常的文档流，它没有脱离文档流，但是它的top/left/
 right/bottom属性是生效的，可以说它是static到absoult的一个中间过渡属性，最重要的是它还占有文档空间，而且`占据的文档空间不会随 top/right/left 
 /bottom等属性的偏移而发生变动，也就是说它后面的元素是依据虚线位置(top/left/right/bottom等属性生效之前)进行的定位`，这点一定要理解。
---

(3) 添加margin属性：
```html
<style type="text/css">
    #first{width: 200px;height: 100px;border: 1px solid red;position: relative;top: 20px;left: 20px;margin: 20px;} /* add margin*/
    #second{width: 200px;height:100px;border: 1px solid blue;}
</style>
```
##### 添加margin属性后：

---
![loading](/img/2018_10_19/margin.png "添加margin属性后")

> 对比一下，是不是就很清晰了，我们先将first元素外边距设为20px，那么second元素就得向下偏移40px，所以margin是占据文档空间！同理，大家可以自己动手测下
padding的效果吧！

---


* #### 绝对定位(absoulte)
```
 absoulte定位，也称为绝对定位，虽然它的名字号曰“绝对”，但是它的功能却更接近于"相对"一词，为什么这么讲呢？原来，使用absoult定位的元素脱离文档流后，就只能根据祖
 先类元素(父类以上)进行定位，而这个祖先类还必须是以postion非static方式定位的， 举个例子，a元素使用absoulte定位，它会从父类开始找起，寻找以position非static
 方式定位的祖先类元素(注意，一定要是直系祖先才算哦~），直到<html>标签为止，这里还需要注意的是，relative和static方式在最外层时是以<body>标签为定位原点的，而
 absoulte方式在无父级是position非static定位时是以<html>作为原点定位。<html>和<body>元素相差9px左右。我们来看下效果：
```

(4) 添加absoulte属性:
```html
<html>
<style type="text/css">
    html{border:1px dashed green;}
    body{border:1px dashed  purple;}
    #first{ width: 200px;height: 100px;border: 1px solid red;position: relative;}
    #second{ width: 200px;height: 100px;border: 1px solid blue;position: absolute;top :0;left : 0;}
</style>
<body>
  <div id="first">relative</div>
  <div id="second">absoult</div>
</body>
</html>
```

##### 添加absoulte属性后：

---
![loading](/img/2018_10_19/absoulte.png "添加absoulte属性后")

>  哈哈，看了上面的代码后，细心的朋友肯定要问了，为什么absoulte定位要加 top:0; left:0; 
属性，这不是多此一举呢？

> 其实加上这两个属性是完全必要的，因为我们如果`使用absoulte或fixed定位的话，必须指定 left、right、 top、 bottom 属性中的至少一个，否则left/right/top/bottom属性会使用它们的默认值auto，这将导致对象遵从正常的HTML布局规则，在前一个对象之后立即被呈递，简单讲就是都变成relative`，会占用文档空间，这点非常重要，很多人使用absolute定位后发现没有脱离文档流就是这个原因，这里要特别注意~~~

>少了left/right/top/bottom属性不行，那如果我们多设了呢？例如，我们同时设置了top和bottom的属性值，那元素又该往哪偏移好呢？记住下面的规则：
* 如果top和bottom一同存在的话，那么只有top生效。
* 如果left和right一同存在的话，那么只有left生效。

---

```
既然absoulte是根据祖先类中的position非static元素进行定位的，那么祖先类中的margin/padding会不会对position产生影响呢？看个例子先：
```
---
(5) 在absoulte定位中添加margin / padding属性：
```html
#first{width: 200px;height: 100px;border: 1px solid red;position: relative;margin:40px;padding:40px;}
#second{width: 200px;height:100px;border: 1px solid blue;position: absolute;top:20px;left:20px;}
   
<div id="first">first
    <div id="second">second</div>
</div>
```
##### 在absoulte定位中添加margin / padding属性：

---
![loading](/img/2018_10_19/absoulte_margin.png "在absoulte定位中添加margin / padding属性")

> 看懂了，祖先类的margin会让子类的absoulte跟着偏移，而padding却不会让子类的absoulte发生偏移。总结一下，就是absoulte是根据祖先类的border进行的定位。

> Note : 绝对(absolute)定位对象在可视区域之外会导致滚动条出现。而放置相对(relative)定位对象在可视区域之外，滚动条不会出现。


* #### 固定定位(fixed)
```
 fixed定位，又称为固定定位，它和absoult定位一样，都脱离了文档流，并且能够根据top、right、left、bottom属性进行定位，但不同的是fixed是根据窗口为原点进行偏移定位的，也就是说它不会根据滚动条的滚动而进行偏移。
```

### Z-index属性
```
 z-index，又称为对象的层叠顺序，它用一个整数来定义堆叠的层次，整数值越大，则被层叠在越上面，当然这是指同级元素间的堆叠，如果两个对象的此属性具有同样的值，那么将依据它们在HTML文档中流的顺序层叠，写在后面的将会覆盖前面的。需要注意的是，父子关系是无法用z-index来设定上下关系的，一定是子级在上父级在下。
```

> 使用static 定位或无position定位的元素z-index属性是无效的。