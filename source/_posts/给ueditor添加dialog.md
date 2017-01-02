layout: w
title: 给ueditor添加dialog
date: 2017-01-02 10:02:04
tags: [ueditor,dialog]
---
> 以添加"guimeisang"按钮为例说明，下面详细讲怎么给UEditor添加自定义弹窗。
> 各位看官看完记得自己写个demo看看咯...

<!--more-->

### 先大概说下UEditor的文件目录  
- dialogs: 弹出对话框对应的资源和JS文件  
- lang: 编辑器国际化显示的文件  
- themes: 样式图片和样式文件  
- third-party: 第三方插件(包括代码高亮，源码编辑等组件）  
- ueditor.all.js: 开发版代码合并的结果,目录下所有文件的打包文件  
- ueditor.all.min.js: ueditor.all.js文件的压缩版，建议在正式部署时采用  
- ueditor.config.js: 编辑器的配置文件，建议和编辑器实例化页面置于同一目录  
- ueditor.parse.js: 编辑的内容显示页面引用，会自动加载表格、列表、代码高亮等样式  
- ueditor.parse.min.js: ueditor.parse.js文件的压缩版，建议在内容展示页正式部署时采用

### 修改ueditor.config.js文件
在ueditor.config.js文件中，找到toolbars参数，增加一个“guimeisang”字符串，对应着添加一个labelMap，用于鼠标移上按钮时的提示。  
```
//工具栏上的所有的功能按钮和下拉框，可以在new编辑器的实例时选择自己需要的从新定义
, toolbars: [[
    'fullscreen', 'source', '|', 'undo', 'redo', '|',
    'bold', 'italic', 'underline', 'fontborder', 'strikethrough', 'superscript', 'subscript', 'removeformat', 'formatmatch', 'autotypeset', 'blockquote', 'pasteplain', '|', 'forecolor', 'backcolor', 'insertorderedlist', 'insertunorderedlist', 'selectall', 'cleardoc', '|',
    'rowspacingtop', 'rowspacingbottom', 'lineheight', '|',
    'customstyle', 'paragraph', 'fontfamily', 'fontsize', '|',
    'directionalityltr', 'directionalityrtl', 'indent', '|',
    'justifyleft', 'justifycenter', 'justifyright', 'justifyjustify', '|', 'touppercase', 'tolowercase', '|',
    'link', 'unlink', 'anchor', '|', 'imagenone', 'imageleft', 'imageright', 'imagecenter', '|',
    'simpleupload', 'insertimage', 'emotion', 'scrawl', 'insertvideo', 'music','audio', 'attachment', 'map', 'gmap', 'insertframe', 'insertcode', 'webapp', 'pagebreak', 'template', 'background', '|',
    'horizontal', 'date', 'time', 'spechars', 'snapscreen', 'wordimage', '|',
    'inserttable', 'deletetable', 'insertparagraphbeforetable', 'insertrow', 'deleterow', 'insertcol', 'deletecol', 'mergecells', 'mergeright', 'mergedown', 'splittocells', 'splittorows', 'splittocols', 'charts', '|',
    'print', 'preview', 'searchreplace', 'help', 'drafts','guimeisang'
]]
//当鼠标放在工具栏上时显示的tooltip提示,留空支持自动多语言配置，否则以配置值为准
,labelMap:{
    guimeisang:'桂梅桑'
}
```

### 修改ueditor.all.js文件
找到iframeUrlMap增加一行：   
```
'audio': '~/dialogs/guimeisang/guimeisang.html',
```

找到btnCmds、dialogBtns增加一个元素：guimeisang   

接下来在dialogs文件夹下创建guimeisang文件夹并新建guimeisang.html     
```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <script type="text/javascript" src="../internal.js"></script>
    <style type="text/css">
        *{margin:0;padding:0;color: #838383;}
        table{font-size: 12px;margin: 10px;line-height: 30px}
        .txt{width:300px;height:21px;line-height:21px;border:1px solid #d7d7d7;}
    </style>
</head>
<body>

    <table>
        <tr>
            <td><label for="href">桂梅桑blog</label></td>
            <td><input class="txt" id="href" type="text" /></td>
        </tr>    
    </table>
<script type="text/javascript">
    function handleDialogOk() {
        if ($G('href').value) {
            var patt1 = /blog/gi;

            if (patt1.test($G('href').value)) {
                editor.execCommand('insertHtml','博客') ;
                dialog.close();
            } else {
                alert("这不是滴！！！");
                return false;
            }

        } else {
            alert("啥都没有");
            return false;
        }
    }
    dialog.onok = handleDialogOk;
    $G('href').onkeydown = function(evt) {
        evt = evt || window.event;
        if (evt.keyCode == 13) {
            handleDialogOk();
            return false;
        }
    }
</script>
</body>
</html>
```
相关的操作js也写在该html里面。  
到这里，运行编辑器 新加的按钮已经出来啦，但是点击弹出的窗口样式不对 乱了；  
这时候，修改themes/default/css/ueditor.css文件，增加一条样式：  
```
.edui-default  .edui-for-audio .edui-icon {
    background-position: -18px -40px
}
```
至此，弹窗可以正常显示了，并能插入相应的代码。
