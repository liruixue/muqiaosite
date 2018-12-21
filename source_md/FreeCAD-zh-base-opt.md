---
title: '3d打印利器FreeCAD入门教程之一----基础操作篇'
date: 2018-12-03 19:46:27
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
- 教程
---

# FreeCAD是什么
FreeCAD是一种通用的3D CAD建模软件，来自法国Matra Datavision公司，是完全开源（GPL的LGPL许可证）易于学习和掌握的常用3d打印建模的工具。需要该软件可以自行从[这里](https://www.freecadweb.org/)进行下载并安装使用，本文写作时，0.18版本即将发布，本文所描述的操作都是基于0.17版本的.

<!-- more --> 

#  概要介绍
FreeCAD接口背后的主要概念是将它分成工作台(workbench)。工作台是适用于特定任务的工具集合，例如使用网格或绘图2D对象或约束草图。您可以使用工作台选择器切换当前的工作台。3d模型制作过程中，经常使用到的是零件设计工作台(Part Design)和零件工作台(Part)。见下图可供选择的workbench列表：
  ![工作台-workbench](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-intro/freecad-workbench-select.jpg)
您将在FreeCAD中开始使用的工作台取决于您需要做的工作类型，比如：
> * 如果要使用机械模型，或者更常用的任何小型对象，则可能需要尝试零件设计（Part Design）工作台。
> * 如果在2D中工作，如果需要约束，则切换到草图工作台(drft)或素描(skecher)工作台
> * 如果想做BIM，则启动建筑工作台。如果您正在使用船舶设计，那么您将会有一个特殊的船舶工作台
> * 如果来自OpenSCAD世界，请尝试OpenSCAD 工作台。

您可以随时切换工作台，还可以通过自定义您最喜欢的工作台来添加其他工作台的工具。甚至可以通过'Tools-AddOn Manager'菜单来添加你喜欢的插件，比如说像螺丝钉螺纹的工作台就是通过插件的方式来添加的.

特别提一点，FreeCAD对中国用户还是比较友好的，提供了**菜单的汉化**，觉得英文菜单不舒服的同学，可以自行通过'Edit-Preference-General-Language-Change Language'来切换到中文.

#  在3D空间中导航
3d空间中导航的操作是你入门首先要掌握的技巧，可见下图；
  ![鼠标操作](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-intro/freecad-mouse-opt.jpg)
    
如果你想用触摸板及手势来操作，FreeCAD也做了相应的响应，请见[这里的详细说明](https://www.freecadweb.org/wiki/index.php?title=Mouse_Model#CAD_Navigation_.28default.29)

如果是键盘操作：
> * CTRL SHIFT + 三键同时按下是放大， CTRL - 同时按下是缩小
> * 键盘的上下左右方向的箭头会让3D对象发生位移
> * SHIFT + 左箭头或右箭头，会让3D对象逆时针或顺时针旋转90度方向
> * 数字键1 到 6，分别代表了6个不同的标准视角：上前右下背左

#  在零件设计工作台和素描工作台中工作
零件设计工作台专门用于构建复杂的对象，从简单的形状开始，添加或删除形状块（我们称之为“特性(feature)”），直到你得到你想要的对象。您在建模过程中应用的所有功能都存储在名为树视图的单独视图中，该视图还包含文档中的其他对象。您可以将PartDesign对象视为一系列操作，每个操作都应用于前一个操作的结果，形成一个大链。在树视图中，您可以看到最终的对象，但可以展开它并检索所有先前的状态，并更改任何参数，这些参数会自动更新到最终对象。
  ![树视图](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-intro/freecad-Partdesign-tree-view.jpg)
  
零件设计(Part Design)工作台大量使用另一个工作台--素描（Skecher）工作台。素描器允许您绘制2D形状，通过将约束应用于 2D 形状来定义，如果你发现你的线条不能动弹的时候，你要想一想是不是要删除多余的约束。例如，您可以绘制一个矩形，并通过将长度约束（Constraints）应用于其中一边来设置边的大小，那么这条边不能再调整大小了（除非修改了约束）。
  ![树视图](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-intro/freecad-skecher-constraints.jpg)

使用素描绘制器制作的2D形状在零件设计工作台中使用很多，例如创建3D立方体，或者绘制对象面上的区域，然后将其从主体空心化。这是一个典型的零件设计工作流程：

1.创建一个新的素描
2.绘制一个封闭的形状（确保所有点都被连接起来）
3.关闭素描
4.使用拉伸工具(工具栏上方的Pad a selected skech)将素描展开成3D立体
5.选择固体的一个面
6.创建一个第二个素描（这一次将在所选的面上绘制）
7.画一个封闭的形状
8.关闭素描
9.从第二个素描，在第一个对象上挖去一个洞
这给你一个这样的对象：
  ![第一个建模操作](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-intro/freecad-Partdesign_example.jpg)  

在任何时候，您可以选择原始素描并进行修改，或更改垫或挤压操作的缩放参数，这将更新最终对象。

>**专题扩展**
关于FreeCAD的一些常见建模的概念及方式的主题，[点击此处](http://limuqiao.com/categories/%E5%88%9B%E5%AE%A2%E5%90%A7/%E5%88%9B%E5%AE%A2%E5%BF%85%E5%A4%87/FreeCAD/)获取更多信息，敬请关注.

    声明-文章部分内容出自英文直译，并由作者本人的亲历及经验总结，欢迎转载并带出原文链接

作者 [@碧海饮冰人]    
2018 年 12 月 3日    


