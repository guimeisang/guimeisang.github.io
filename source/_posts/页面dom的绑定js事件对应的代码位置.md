---
title: 页面dom的绑定js事件对应的代码位置
date: 2017-04-08 18:09:42
tags:
---

### 页面dom的绑定js事件对应的代码位置

背景：平时我们在调试的时候想知道页面上的某一个dom上绑定了什么事件以及绑定代码的位置，下面就介绍三种跟踪页面dom的事件的方法。  

<!--more-->

1. 使用firefox调试。  
   我们可以使用firefox的debug工具，找到指定的元素，然后查看事件面板  
    ![image](http://oi9n0t0p1.bkt.clouddn.com/blog/201704jsdebug1.png)   

2. 使用chrome调试。
   在检查的元素上查看面板，在EventListeners标签，可以看到相关事件的绑定信息，点击相应的文件可以跳转到事件定义代码在文件中的位置。  
   ![image](http://oi9n0t0p1.bkt.clouddn.com/blog/201704jsdebug2.png)


3. 使用chrome web store中的Visual Event检查事件绑定信息   
   上面两个方法，当我们定位事件代码的位置时，如果我们使用js库（比如jquery）的话，调试工作会变得复杂，程序往往会讲我们引导到jquery库中，这样的话仍然是不方便找到哪个文件中addEventListener的事件、这样就可以安装Visual Event。  
   安装完 Visual Event 后，工具条上会有 Visual Event 的图标。然后打开我们要调试的页面，在工具栏上点击他那个火眼金睛一样的眼睛图标，网页上所有绑定了事件的 HTML 元素都会由一个半透明的蓝色遮罩层覆盖，鼠标移动到相应的元素上即可看到事件绑定信息。  
   刚才说了，在使用了Js 库的时候，visual event 依然很好用，下面列出它支持的几个库的名字及版本信息：  

获取Visual Event：  
- VisualEvent 在GitHub上的位置 ：https://github.com/DataTables/VisualEvent
- VisualEvent 在Chrome webstore 上的位置：https://chrome.google.com/webstore/detail/visual-event/pbmmieigblcbldgdokdjpioljjninaim   

4. 如果都没有办法，则需要在js文件中不断的debug和梳理js逻辑  
