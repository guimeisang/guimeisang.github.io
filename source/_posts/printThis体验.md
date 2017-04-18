---
title: printThis体验
date: 2017-04-18 10:18:48
tags: jQuery插件 
---
> jQuery pringting plugin; print specific elements on a page; \
> 自定义打印的jQuery插件

<!--more-->

```
$("#mySelector").printThis({
    debug: false,               // show the iframe for debugging
    importCSS: true,            // import page CSS
    importStyle: false,         // import style tags
    printContainer: true,       // grab outer container as well as the contents of the selector
    loadCSS: "path/to/my.css",  // path to additional css file - use an array [] for multiple
    pageTitle: "",              // add title to print page
    removeInline: false,        // remove all inline styles from print elements
    printDelay: 333,            // variable print delay; depending on complexity a higher value may be necessary
    header: null,               // prefix to html
    footer: null,               // postfix to html
    base: false ,               // preserve the BASE tag, or accept a string for the URL
    formValues: true,           // preserve input/form values
    canvas: false,              // copy canvas elements (experimental)
    doctypeString: "...",       // enter a different doctype for older markup
    removeScripts: false        // remove script tags from print content
});
```

坑点：
1. css的样式，可以采用媒体查询设置打印的样式， @media print {...}   
2. 可以同事打印几个dom   

```
$('#kitty-one, #kitty-two, #kitty-three').printThis({
    importCSS: false,
    loadCSS: "",
    header: "<h1>Look at all of my kitties!</h1>"
});
```