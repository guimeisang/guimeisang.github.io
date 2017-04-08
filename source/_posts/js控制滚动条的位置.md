---
title: js控制滚动条的位置
date: 2017-04-08 18:08:45
tags: js小效果
---

场景： 控制dom元素内的滚动条，需要控制滚动条的位置。

<!--more-->

**HTML**
```
<div class="scroll-wrap" style="overflowY:scroll">
    <div id='div1'>1</div>
    <div id='div2'>2</div>
    <div id='div3'>3</div>
    <div id='div4'>4</div>
    <div id='div5'>5</div>
    <div id='div6'>6</div>
    <div id='div7'>7</div>
    ....
    <div id='div100'>100</div>
</div>
```

**js**
```
(function($){
    $('div.scroll-warp').scrollTop = $('#div50').offsetTop;
})();
```

注意这样的话可以通过计算得到需要移动的距离，具体可以参考下面这个图   

![image](http://oi9n0t0p1.bkt.clouddn.com/blog/201704/scroll.png)