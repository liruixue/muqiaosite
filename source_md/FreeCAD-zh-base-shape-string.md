---
title: '3d打印利器FreeCAD入门教程之三----3d文字雕刻篇'
date: 2018-12-10 19:28:28
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
讲述3D打印中的模型，如何添加文字的雕刻效果,及FreeCAD的相关ShapeString, extrude等工具的应用技巧


#  利用ShapString工具雕刻文字的过程

先看利用ShapString工具挖空后效果图
  ![字体挖空效果](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-shape-str/shapeString-completed-preview.jpg)
<!-- more --> 
然后我们再一步步往后看

## 创建基本形状
我们要先来创建一个待雕刻的立方体:
- [ ] 启动freecad创建新文档，切换至‘part’工作台
- [ ] 通过点击工具栏'create a cube solid'插入一个立方体
- [ ] 确保树视图中的立方体被选中，通过数据选项卡中的属性视图，将width改为31毫米，
现在，我们可以画一个矩形，通过选择矩形工具，点击2个角点。你可以在任何地方把两个点放下，然后对应的地方就会被放置这个矩形。
- [ ] 点击'chamfer the selected edges of a shape'，然后选择Edge6，指定‘5mm’的length后关闭'tasks'选项页
 
立方体的效果如下图
  ![ ](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-shape-str/chamfer-BasicShape.jpg)

  
## 使用‘Draft’工作台的‘Shape String’工具来插入文本
- [ ] 切换到‘Draft’工作台
- [ ] 确保‘tree view’视图没有选中任何物体，触发工具栏上的‘Current working plane’按钮并选择XY (Top)
- [ ] 点击工具栏的‘S’型图标(Creates text string in shapes)来创建文字形状，后面一路修改输入值一面回车（Enter）往下走就行
- [ ] 接下来的输入框中，“Global X/Y/Z”均改成'0' ,字串就填'FreeCAD',height填5mm，tracking默认值0，字体输入框需要去选字体文件，直接去C://windows/fonts下选取自己喜爱的字体，如果不能操作那个目录可以拷贝一份出来再选取

## 创建3D效果的文本
- [ ] 切换回part工作台
- [ ] 确保树视图中的 "Shapestring" 对象被选中（可以通过space键来切换对象的显示和隐藏来确认是否选中）
- [ ] 使用凸台工具（Extrude a selected sketch）来将文本变成3D形体，参数Along=1mmm,选中'Create a solid'项，点击‘ok’完成
 

  ![ ](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-shape-str/draftShapeString-extrude.jpg)

## 插入定位用的sketch图形
- [ ] 切换至‘Sketcher’工作台
- [ ] 确定"chamfer"对象在tree view中被选中（space键控制显示和隐藏来确认）
- [ ] 选中之前‘chamfer’操作中出现的那个斜面
- [ ] 点击工具栏上的‘create a new sketch’来插入一个新的sketch，当然这个sketch是基于刚刚被选中的斜面来绘制的
- [ ] 画一条横线，像下图那样 (水平的就行，长度不重要 ...)，然后根据下图中的示例来给定相应的距离约束
 

  ![ ](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-shape-str/positionsketch-constraints.jpg)


## 在3D空间里给3D文本进行定位
- [ ] 确保"Extrude"在tree view中被选中（space键控制显示和隐藏来确认）
- [ ] 属性中的data选项页中点击Placement项的value栏，点击'...'的按钮切换去‘Tasks’标签页去更新位置信息
- [ ] 选中Apply incremental changes to object placement项，然后Axis下拉框中设置Z=90°，Y=45°， 点击ok完成
- [ ] 切换回‘Draft’工作台，工具栏中的'Draw Style'切换到‘Wireframe’模式
- [ ]  选中"Extrude"对象，点选工具栏中的十字形的move工具，在3d视图下选中‘Etrude’对象的左上角，再点击sketch上的那个约束点，就可以完成字体对斜面的定位
 

  ![ ](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/freecad-shape-str/shapeString_Position.jpg)

  
## 创建雕刻的文本
- [ ] 切换回'Part'工作台，使用"As is"视图模式
- [ ] tree view上选中' Chamfer'对象，然后再选中3D 文本("Extrude")
- [ ] 点击’‘进行布尔运算来减去文字的轮廓
- [ ] 按Space键将 "Sketch" 隐藏掉
- [ ] 一个被雕刻的斜坡长方体已经完成，效果图可以见文首的图片
 
**后续扩展**
> * 再加一个补充说明，刚才我们插入的字体是英文的，如果要用中文也是可以的，只需要找到你所需要的中文字体文件加载进来，这些都可以在你已经完成的ShapeString对象属性里进行更改，马上就可以看到相应的效果出现.
> * 关于FreeCAD的一些常见建模的概念及方式的主题，[点击此处](http://limuqiao.com/categories/%E5%88%9B%E5%AE%A2%E5%90%A7/%E5%88%9B%E5%AE%A2%E5%BF%85%E5%A4%87/FreeCAD/)获取更多信息，敬请关注.

    声明-文章部分内容出自英文直译，并由作者本人的亲历及经验总结，欢迎转载并带出原文链接

作者 [@碧海饮冰人]    
2018 年 12 月 10日    


