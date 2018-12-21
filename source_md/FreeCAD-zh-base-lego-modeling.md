---
title: '3d打印利器FreeCAD入门教程之二----乐高积木块建模操作篇'
date: 2018-12-04 19:48:28
categories:
- 创客吧
- 创客必备
- FreeCAD
tags:
- Arduino
- 进阶
- 入门项目
- 3d打印
- FreeCAD
- 文字雕刻
- 教程
---

# 本节内容
解释什么是建模，通过乐高积木块的建模过程来带你熟悉PartDisign工作台中常用的诸如创建Sketch，External Geometry导入边缘线，建立约束，PAD，Extrude，Linear Pattern等常用操作

# 建模是要干什么
3D打印产生的是真实世界的物体，要让画纸上的东西走向真实世界，就得要求你设计的图纸是可以建造的，而且模型必须是实心的，这样才可以被3D打印机所识别并打印出来。在FreeCAD工具里‘PartDesign Workbench’则完美地解决了这个‘实心建造’的问题。
为了展示如何使用PartDesign Workbench，我们可以从大伙比较了解的乐高（lego）积木块的模型来进行演示。
  ![乐高积木块的效果](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/lego-summary.jpg)
<!-- more --> 

#  乐高（Lego）组件建模的过程
这次我们要建一个乐高模型块，大概是实际lego产品的1.5倍大小。主要用了Sketcher及PartDesign工作台上的相关工具。PartDesign的对象完全是依赖Sketches(草图)而存在的，每一个Skecher都是2D的对象，这些对象主要由一些线（线段，弧线，圆形线等）及其Constraints（约束)来构成。对于2D对象中存在的约束条件，我们追求的原则是尽量减少冗余的条件，FreeCAD系统会自动发现这些冗余的约束，以便于你及时地去删除它们。Sketch（草图）有个编辑模式，进入这个模式可以改变草图中的几何形状和约束条件，当你完成了编辑，需要去主动退出(Close)这种编辑模式。

## 乐高立方体的生成
我们要先来建模一个立方形状，这将我们的乐高砖块的基础。稍后我们将来在其上镂空，并添加8个卡扣点。先让我们开始通过一个矩形草图，然后将其凸起:
> * 先切换到PartDesign工作台
> * 单击Create a new Sketch按钮。将会出现一个对话框，询问你想所在的sketch，选择XY平面，sketch会被创建（如果sketch没有Active Body，可能会要求你创建一个body）并立即切换到编辑模式，视图也将旋转到你的Sketch图形正面。
> * 现在，我们可以画一个矩形，通过选择矩形工具，点击2个角点。你可以在任何地方把两个点放下，然后对应的地方就会被放置这个矩形。
> * 会发现有几个约束已经自动添加到我们的矩形：垂直段有一个垂直约束，水平的受水平约束限制，每一个角落有point-on-point约束把它们粘在一起。你可以通过拖动鼠标来移动矩形，所有的几何形体将继续遵守约束。
  ![乐高积木块的轮廓草图](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skecher1.jpg)

现在，让我们添加三个约束:
> * 选择一个垂直段和添加一个垂直距离约束，给它一个23.7毫米的大小
> * 选择一个水平段和添加一个水平距离约束47.7毫米。
> * 最后，选择一个角点，然后起源点(点在红色和绿色的交叉轴，即XY轴的交叉点)，然后添加一个点对点的一致约束。矩形将跳转到起源点，然后你的Skech会变绿，这意味着它被完全约束，你可以试着去移动线或点，但它却不会有什么变化。
  ![乐高块轮廓固定](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skecher_fixed.jpg)

注意，最后的那个point-on-point约束并不是绝对必要的，你倒不是完全必须使用完全约束草图。然而，如果我们要打印3d物体，会需要我们将物体接近原点的那个点(那里是打印机头可以移动空间的中心)。通过添加约束，我们是要确保我们的作品将始终保持“锚定”在起源点上。

基础素描现在准备好了，我们可以按下任务面板上的关闭按钮，或简单地按Escape键来退出编辑模式。以后如果有需要，我们可以随时通过双击树视图中的Sketcherl来重新回到编辑模式。

然后使用凸台工具（点击工具栏上'Pad a selected ketch'），并给它一个14.4mm的距离。其他选项可以保留为默认值
  ![乐高块被挤出高度](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skecher_pad.jpg)
pad的行为很像Part工作台里的Extrude工具，但有一些差异，主要差别之一就是pad出来的对象无法移动，它永远是附着在草图上。如果你想改变pad的位置，你必须移动基础sketch。在当前情况下，我们想确保什么都不会离开原来的位置，这样是比较安全的做法。

## 乐高立方体的挖空
现在我们将来雕刻这个乐高块，用得是Pocket(凹陷)工具，这个工具相当于是Part工作台的的Cut功能在PartDesign版本的实现。为了创建一个Pocket(凹陷)，我们将创建一个底部表面上的sketcher，这个是用于删除乐高块中的一部分。
选择底面，按'Create a new sketch'按钮。
FreeCAD会把视图自动切换去底面，这时候可以在上面画一个矩形
现在我们将约束矩形与底部面的关系，要做到这一点，我们需要使用'Create an edge linked to an external geometry'工具来“导入”一些参考面上的边。使用这个工具点击底面的两个竖线边，引用的边则会变成紫红色作为参考。如下图这般：
  ![乐高块上挖空的Sketch](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_pocket.jpg)

External Geometry(外部几何)不是“真的”，当我们离开编辑模式之后它将会被隐藏起来。但是我们可以用它来建立约束，有以下四处约束:
> * 选择矩形的左上角点和参考线左上角的点，添加一个1.8mm的水平距离约束
> * 再次选择矩形的左上角点和参考线左上角的点，添加一个1.8mm的垂直距离约束
> * 选择矩形的右下角点和参考线右下角的点，并添加一个1.8mm的水平距离约束
> * 选择矩形的右下角点和参考线右下角的点，并添加一个1.8mm的垂直距离约束
离开编辑模式，我们现在可以选中工具栏上的'Create a pocket with a selected sketch'按钮进行操作。给它一个12.6mm的长度，这将离开Pad上表面的厚度为1.8mm(记住，垫的总高度是14.4mm哦)。
  ![乐高块上挖空效果](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_pocket_end.jpg)

## 乐高立方体上面八个点的生成
现在我们将搞定在顶面上的8个点。要做到这一点，考虑到他们是相同的重复特性，我们将使用方便的PartDesign工作台上的Linear Pattern（线性模式）工具，它将允许一次建模，然后将模型重复：
> * 首先，选择乐高块的顶面
> * 创建一个新的Sketch。
> * 创建两个圆圈。
> * 对于每一个圆，选择它，并添加一个3.6mm的半径约束
> * 使用'Create an edge linked to an external geometry'工具导入乐高顶面的左边缘线。
> * 对每个圆的中心点加入两垂直约束和两个水平约束，距离都是离顶面两条边为6mm
  ![乐高块顶面两圆圈](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_two_circles.jpg)

> **注意**，再一次声明，当你锁定了sketch的位置和尺寸就是**完全约束的**，这种做法会比较安全，即使你现在改变了最初的那个sketch，我们之后的sketch的也会随之进行确定的变化。

离开编辑模式，选择这个新的sketch，来创建一个2.7毫米的Pad。
  ![乐高块顶面两点出现](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_two_circles_pad.jpg)

> 注意，与早些时候使用Pocket工具一样，因为我们使用了我们的基本块的顶面作为最新的Sketch基面，那我们所做的任何PartDesign操作，这个sketch将正确地建立在基面上:它们并不是独立的对象，它们是从乐高立方体而来。这是使用PartDesign工作台的一个巨大的优势，只要你走的每一步是基于前一个步骤构建而来，实际上你最终就可以得到一个坚固的对象。

我们现在可以重复建造前面凸出的那两个点四次，这样我们就可以得到八个点，选择我们刚刚创建的最新的Pad。
按'Create a linear pattern feature'(线性模式)按钮。
给它一个36mm的长度(总“跨度”是指所以copy模型间的总距离)，“Horizontal sketch axis”方向，并使其出现4次:
  ![乐高块顶面八点出现](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_eight_circles_pad.jpg)
  
现在我们要用三根“管”来填补我们之前挖空的空间。我们有几种方式可以做:创建一个带三个圈的sketch，pad他们然后再pocket三次；或者创建一个内部还带有一个圆圈的sketch，然后pad这个管子形状的sketch；或是其他组合方式来完成这个目标。FreeCAD有时候是有很多方法可以实现相同的结果。有时我们希望的方式不能达到目的，我们就必须尝试其他方式。在这里，我们将采取最稳妥的方式，一步一个脚印做好了。
> * 选择底部我们之前挖好的中空的那个面，创建一个新的sketch，添加一个半径为4.8825 mm的圆，导入参考基面的左边界线，并限制垂直和水平在10.2mm.
> * 离开编辑模式，pad这个sketch，高度是12.6mm
> * 用新创建的PAD来创建一个Linear Pattern， 跨度是24mm， 并出现三次.
到现在，我们现在有三个实心的管，再让我们做出最后的洞
> * 选择刚才创立三个管子中的第一个，创建一个新的sktch
> * 导入圆形面的边界，创建一个半径约束3.6mm的圆，并添加一个对圆心点对点之间的约束，这样我们现在有一个完美的完全约束的中心圈
> * 离开编辑模式，并用Pocket工具从这个sketch创建一个深度为12.6mm的洞
> * 同样地，还是创建一个跨度24mm，出现三次的Linear Pattern。
  ![乐高块空心管制作](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_fufilled_tube.jpg)

恭喜，乐高块的制作现在已经完成了，喜欢的话，像下图一样还可以在property view的view标签页里给它一个漂亮的颜色来宣告下自己的胜利:)
   ![乐高块完成效果](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-lego-modeling/Exercise_lego_skech_completed_effect.jpg)

   
>**后续扩展**
关于FreeCAD的一些常见建模的概念及方式的主题，[点击此处](http://limuqiao.com/categories/%E5%88%9B%E5%AE%A2%E5%90%A7/%E5%88%9B%E5%AE%A2%E5%BF%85%E5%A4%87/FreeCAD/)获取更多信息，敬请关注.

    声明-文章部分内容出自英文直译，并由作者本人的亲历及经验总结，欢迎转载并带出原文链接

作者 [@碧海饮冰人]    
2018 年 12 月 4日    


