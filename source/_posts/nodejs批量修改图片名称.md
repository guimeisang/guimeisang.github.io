---
title: nodejs批量修改图片名称
date: 2017-01-10 14:14:37
tags: nodejs 批量修改图片名称
---
### nodejs批量修改图片名称

首先看下文件目录结构  
+img(files)    
+changeName.js   

我们要将img文件夹中的图片改成abc1，abc2,...之类名字的图片  

然后我们看看changeName.js的内容吧   

```
// 引入fs文件处理模块
var fs = require("fs");
var path = 'img'
fs.readdir(path, function(err, files) {

    // files是名称数组
    files.forEach(function(filename,index) {
        //修改图片名称，当然如果只是改名字中一部分也是可以的，用字符串的拼接就好  
        var oldPath = path + '/' + filename,
        newPath = path + '/' + 'module' + index;
        //newPath = path + '/' + filename.replace(/aa/g,'bb');
        fs.rename(oldPath, newPath, function(err) {
            if (!err) {
                console.log(filename + '替换成功!')
            } 
        })
    })
})            
```

然后在changName.js的目录下面用命令行运行下`node changeName.js` 就可以了
 
