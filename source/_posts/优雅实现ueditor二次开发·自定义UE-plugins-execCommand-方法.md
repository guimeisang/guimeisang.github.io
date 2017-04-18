---
title: 优雅实现ueditor二次开发·自定义UE.plugins&&execCommand()方法
date: 2017-04-18 11:42:22
tags: ueditor二次开发，ueditor自定义plugins和execCommand方法
---
> ueditor优雅的实现方式，并且功能多样，性能不错。当然我们在使用ueditor进行二次开发的时候，会遇到很多它没有的功能。下面就是主要介绍怎么去优雅的二次开发ueditor。  

<!--more-->

### 自定义UE.plugins  

ueditor会将一些属性和方法都绑定在命名空间上，一般会将UE，editor，ue绑定在window下面，所以我们在[ueditor demo](http://ueditor.baidu.com/website/onlinedemo.html)控制命版上就可以看到UE的结构。如下图：

![ueditor demo](http://oi9n0t0p1.bkt.clouddn.com/2017/04/ueditorDemo.png)

在二次开发中，一般的思路是先定义方法，然后就是使用该方法。可以在图中看到UE下面有plugins对象，里面包括后退，恢复，设置行高等方法，如下图：  

![UE plugins](http://oi9n0t0p1.bkt.clouddn.com/2017/04/ueplugins.png)

如果在二次开发中，成功添加添加plugins之后，能够在控制平台plugins对象中看到我们自定义的方法，说明添加成功！  

**体现优雅**：可以在在另外一个文件夹中（取名“_custom_example”），把一些需要自定的js，css文件放置在其中，有利于后期维护（**建议不要改动源码，除非实在没办法**）;文件里面的注释也是必不可少的，比如：文件名称，文件版本，文件中方法，方法的参数，方法的返回等；  
**注意**： 在页面记得引入改js文件，并且在editor.all.min.js之后；  

**比如添加全选功能，并且alert('已经全选了'),即使它原来有改方法也没有关系，因为直接可以覆盖掉！**


```
/**
 * selectall(全选)
 * @file selectall.js
 * @description 覆盖 selectall 插件
 * @method editor.execCommand( 'selectall' );
 * @result 全选选取的内容
 */
 UE.plugins['selectall'] = function () {
    var me = this;
    me.commands['selectall'] = {
        execCommand: function() {
            var me = this, body = me.body,
                range = me.selection.getRange();
            var pgBodys = body.getElementByTagName('p');/*选中什么标签，看下你的editor编辑页面有哪些dom*/  
            range.setStartAtFirst(pgBodys[0].firstChild).setEndAtLast(pgBodys[pgBodys.length-1].lastChild)
        },
        notNeedUndo: 1
    };
    
    // 快捷键
    me.addshortcutkey({
        "selectAll":"ctrl+65"
    });
 
 }
 
```

这样就算定义完selecall的模块，相当于已经可以使用editor.execCommand('selectall')；
