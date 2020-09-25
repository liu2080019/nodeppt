title: flutter
speaker: 刘敏
plugins:
    - echarts

<slide class="bg-black-blue aligncenter"  image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">

# flutter {.text-landing.text-shadow}

By 刘敏 {.text-intro}

[:fa-github: Github](https://github.com/ksky521/nodeppt){.button.ghost}


# Flutter弹性布局

> 弹性布局让子组件按照一定比例来分配父容器。弹性布局的概念在其它UI系统中也都存在，如H5中的flex，Android中的FlexboxLayout等。Flutter中的弹性布局主要通过Flex和Expanded来配合实现。

<slide class="bg-black-blue aligncenter" >

## 1.RenderFlex

````dart
RenderFlex({
    List<RenderBox> children,
    Axis direction = Axis.horizontal,
    MainAxisSize mainAxisSize = MainAxisSize.max,
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
    TextDirection textDirection,
    VerticalDirection verticalDirection = VerticalDirection.down,
    TextBaseline textBaseline,
    Clip clipBehavior = Clip.none,
  });
````
> 默认情况下，flex布局是水平的，主轴方向是起点方向，交叉轴方向是居中

<div align=center>

![](http://imgcom.static.suishenyun.net/birthday/aed8729e-9f4e-469c-82e5-1521b2cd5e07.jpg)
</div>


## 1.1 direction属性

> direction属性决定主轴的方向(子节点在主轴的排列方向)

````dart
enum Axis {
  /// 水平方向为主轴
  horizontal,
  /// 竖直方向为主轴
  vertical,
}
````

## 1.2 TextDirection

> TextDirection决定了水平方向的排列方向

````dart
enum TextDirection {
  /// 从左到右
  ltr,
  /// 从右到左
  rtl,
}
````
<div align=center>

![-w415](http://imgcom.static.suishenyun.net/birthday/f921fc86-5dd8-4cae-a238-d51a175bfbc0.jpg)

</div>


## 1.3 VerticalDirection
> VerticalDirection决定了竖直方向的排列方向

````dart
enum VerticalDirection {
  /// 从下到上
  up,
  /// 从上到下
  down,
}
````
<div align=center>

![-w264](http://imgcom.static.suishenyun.net/birthday/59a15a8a-eb94-4236-b47a-a75b3b8c0486.jpg)

</div>


## 1.4 TextBaseline
> TextBaseline用于对齐文本的基准线

````
enum TextBaseline {
  /// 设置为字母类字符的基准线
  alphabetic,
  /// 设置为表意类字符的基准线（中文）
  ideographic,
}
````
<div align=center>

![](https://i.stack.imgur.com/s0ciC.png)
</div>
<div align=center>

![](https://i.stack.imgur.com/b5x6Y.png)
</div>


## 1.5 MainAxisAlignment
> 主轴上子元素的布局方式

````dart
enum MainAxisAlignment {
  /// 左对齐 | 上对齐
  start,
  /// 右对齐 | 下对齐
  end,
  /// 居中对齐
  center,
  /// 两端对齐，子元素之间间隔相等
  spaceBetween,
  /// 每个子元素两侧的间隔相等，所以，子元素之间的间隔比子元素与边框的间隔大一倍
  spaceAround,
  /// 均匀排列每个元素，每个元素之间的间隔相等
  spaceEvenly,
}
````


## 1.6 MainAxisSize

> 主轴所占用的空间

````dart
enum MainAxisSize {
  /// 压缩主轴子元素之间的空隙，使主轴尽可能的短
  min,
  /// 使主轴达到最长
  max,
}
````



