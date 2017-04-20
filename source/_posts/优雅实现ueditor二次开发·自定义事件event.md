---
title: 优雅实现ueditor二次开发·自定义事件event
date: 2017-04-20 12:44:21
tags: ueditor二次开发 自定义事件event
---
> ueditor优雅的实现方式，并且功能多样，性能不错。当然我们在使用ueditor进行二次开发的时候，会遇到很多它没有的功能。下面就是主要介绍怎么去优雅的二次开发ueditor。 
> 自定义事件event

<!--more-->  

**需求场景：** 在二次开发插件时，会发现有些地方，有些地方需要自定一个事件并且对应的函数。（其实回顾一下前端开发，都是各个事件组成，并且在这个事件发生时特定去做什么，这样就可以完成一般的需求了！）  

**ueditor的关于事件的设计（\_src\core\EventBase.js中）**  

可以先去下载ueditor项目的`\_src\core\EventBase.js`中看下源码
   
提炼一下就是：  
- 不得不说下注释：   

```
/**
 * UE采用的事件基类
 * @file
 * @module UE
 * @class EventBase
 * @since 1.2.6.1
 */
 
 /**
 * UEditor公用空间，UEditor所有的功能都挂载在该空间下
 * @unfile
 * @module UE
 */
 
 /**
 * UE采用的事件基类，继承此类的对应类将获取addListener,removeListener,fireEvent方法。
 * 在UE中，Editor以及所有ui实例都继承了该类，故可以在对应的ui对象以及editor对象上使用上述方法。
 * @unfile
 * @module UE
 * @class EventBase
 */
 
 /**
 * 通过此构造器，子类可以继承EventBase获取事件监听的方法
 * @constructor
 * @example
 * ```javascript
 * UE.EventBase.call(editor);
 * ```
 */
 
 /**
     * 注册事件监听器
     * @method addListener
     * @param { String } types 监听的事件名称，同时监听多个事件使用空格分隔
     * @param { Function } fn 监听的事件被触发时，会执行该回调函数
     * @waining 事件被触发时，监听的函数假如返回的值恒等于true，回调函数的队列中后面的函数将不执行
     * @example
     * ```javascript
     * editor.addListener('selectionchange',function(){
     *      console.log("选区已经变化！");
     * })
     * editor.addListener('beforegetcontent aftergetcontent',function(type){
     *         if(type == 'beforegetcontent'){
     *             //do something
     *         }else{
     *             //do something
     *         }
     *         console.log(this.getContent) // this是注册的事件的编辑器实例
     * })
     * ```
     * @see UE.EventBase:fireEvent(String)
     */

```
第一个注释：解释这个文件的作用，并且从所属的模块，类名，第几个版本开始存在。  
第二个注释：解释公共空间。  
第三个注释：解释关于继承该类，从而获得方法的方式。Editor和ui实例都继承了该类。  
第四个注释：解释这个一个构造器，UE.EventBase.call(editor);  
第五个注释：解释注册事件监听器，包含了参数解释，例子；

- 设计思想：  

先构造一个类：
```
var EventBase = UE.EventBase = function () {};
```   
对该类添加事件：  

```
EventBase.prototype = {
    addListener：function(types,listener){},
    on:function(types,listener){},
    off:function(types,listener){},
    trigger:function(){},
    removeListener:function(types,listener){}
}
```  
也就是说可以添加事件，也可以移除事件，另外就是触发事件。  

**二次开发中需要添加事件和触发事件**

使用上面提供的方法，先定义事件，后触发事件。这样在二次开发中就可以自如对应事件，且比较优雅。  


```
/*
*@example 
*@addListener  在加载js之前自定义事件
*@fireEvent    在合适的时候触发事件
*/
editor.addListener('beforeevent',function(){
    console.log('beforeevent');    
})

editor.fireEvent('beforeevent');
```

