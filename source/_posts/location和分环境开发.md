---
title: location和分环境开发
date: 2017-05-19 17:29:34
tags: location 环境配置
---
#### Location对象
- Location 对象包含有关当前URL的信息。
- Location 对象是Window对象的一部分，可通过window.location属性来访问。

##### 对象属性（可以获取和设置）：
- hash：#后面的部分；
- host：主机名和端口；
- hostname: 主机名；
- href：完整的url；
- pathname: url路径名；
- port：url服务器使用的端口号；
- protocol：协议（http,https）;
- search：从问号（？）开始的URL（查询部分）；

##### 对象方法
- assign() 重新加载文档；
- reload() 重新加载当前文档；
- replace() 用新的文档替换当前文档；


在开发中一般会有dev,test,pre,master四种分支，所以接口也会有四种不同的形式，这个时候就可以根据location来做。根据不同路径进行判断不同的环境！

<!--more-->

```
var loaction_origin = location.origin;

var API_ENV = {
    DEV:'http://dev.abc.com/api',
    TEST:'http://test.abc.com/api',
    PRE:'http://pre.abc.com/api',
    MASTER:'http://abc.com/api'
}

var FE_ENV={
    DEV:'http://dev.abc.com',
    TEST:'http://test.abc.com',
    PRE:'http://pre.abc.com',
    MASTER:'http://dec.abc.com'
}

var API_URL = API_ENV.DEV;

if(location.href.indexOf(FE_ENV.TEST) > -1){
    API_URL = API_ENV.TEST;
}else if(location.href.indexOf(FE_ENV.PRE) > -1){
    API_URL = API_ENV.PRE;
}else if(location.href.indexOf(FE_ENV.MASTER) > -1){
    API_URL = API_ENV.MASTER;
}

// 一般都会将该段代码放在common.js里面，配置了这个每次切换环境就不需要每次都改了。

```