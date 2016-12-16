---
title: 一种需要返回ID的下拉框插件
date: 2016-12-16 11:59:50
tags: component
---
### 先看效果
![image](http://oi9n0t0p1.bkt.clouddn.com/dropdown.png)

### HTML
```
 <label class="self_label_class common_label_class"></label>
 <ul class="self_ul_class common_ul_class" style="display:none;"></ul>


 <script src="./custom/component/component.js"></script>
 <script src="./custom/component/dropDown.js"></script>
 <script src="./custom/component/demo.js"></script>

```

### CSS
```
.common_label_class{display:inline-block;width:270px;line-height:42px;height:42px;overflow:hidden;font-size:14px;color:#5a5a5a;border:1px solid #e3e3ed;padding:0px 24px 0px 14px;margin-right:8px;background: url(".") 278px center no-repeat;}
.common_label_class:hover{background: url(".") 278px center no-repeat;border:1px solid #98bd31;}
.common_label_class.active{background: url(".") 278px center no-repeat !important;border:1px solid #98bd31 !important;}
.common_ul_class{position: absolute;width:310px;left:-32px;margin-top:-3px;background:#fff;max-height:297px;overflow-y: auto;}
.common_ul_class li{font-size:12px;color:#666;padding:0px 14px;line-height:32px;border-bottom:1px solid #cdcdd1;border-left:1px solid #cdcdd1;border-right:1px solid #cdcdd1;background:#fff;cursor: pointer;}
.common_ul_class li:hover{background:#f0f0f6;}
```

### demo.js
```
 var typeId = null;//这是点击之后返回的值
 var options = {
     self_label_class:'self_label_class',
     self_ul_class:'self_ul_class',
     common_label_class:'common_label_class',
     common_ul_class:'common_ul_class',
     defaultTxt:"这里写选择框里的内容",
     id:"code",
     name:"codeValue",
     list_data:[ {
         "code" : 1,
         "codeValue" : "易错题"
         },{
         "code" : 2,
         "codeValue" : "典型题"
         }, {
         "code" : 3,
         "codeValue" : "其他"
     }],
     callback:function(currentLiId){
         typeId = currentLiId;
     }
 };
 custComponent.dropDown.init(options);
```

### 组件
```
// dropDown.js
(function(CustomComponent){
    var newLi,_ul,currentLi,currentLiId;

    //初始化组件dom
    function _init(options){
        var name = options.name,
            id = options.id,
            list_data = options.list_data,
            callback = options.callback,
            defaultTxt = options.defaultTxt,
            self_label_class = options.self_label_class,
            self_ul_class = options.self_ul_class,
            common_label_class = options.common_label_class,
            common_ul_class = options.common_ul_class,
            node_label = document.getElementsByClassName(self_label_class)[0],
            node_ul = document.getElementsByClassName(self_ul_class)[0],
            all_node_label = document.getElementsByClassName(common_label_class),
            all_node_ul = document.getElementsByClassName(common_ul_class);
        node_label.innerHTML = defaultTxt;
        for(var i=0;i<list_data.length;i++){
            newLi = document.createElement('li');
            newLi.innerHTML = list_data[i][name];
            newLi.setAttribute("objId",list_data[i][id]);
            newLi.setAttribute("class","dropDown-content-li");
            node_ul.appendChild(newLi);
        }
        _UlBingClick(node_label,node_ul);
        _LiBingClick(node_label,node_ul,options,callback);
        _closeAllDropDown(all_node_label,all_node_ul);
    };

    //点击label显示ul
    function _UlBingClick(node_label,node_ul){
        node_label.onclick = function(event){
            if(node_ul.style.display=="block"){
                node_ul.style.display="none";
            }else{
                node_ul.style.display="block";
            }
            if(event&&event.stopPropagation){
                event.stopPropagation();
            }
        }
    }

    //点击li并且传值过去
    function _LiBingClick(node_label,node_ul,options,callback){
        node_ul.onclick = function(event){
            currentLi = event.target;
            if(currentLi.className == "dropDown-content-li"){
                _currentLiId = currentLi.getAttribute('objId');
                node_label.innerHTML = currentLi.innerHTML;
                if(node_label.innerHTML!= options.defaultTxt){
                    var _className = options.self_label_class +" " + options.common_label_class+" " + "active";
                    node_label.setAttribute('class',_className);
                }else{
                    var _className = options.self_label_class +" " + options.common_label_class;
                    node_label.setAttribute('class',_className);
                }
                node_ul.style.display="none";
                callback(_currentLiId);
                if(event&&event.stopPropagation){
                    event.stopPropagation();
                }
            }
        }

    };

    //如果不是点击在ul或者label上则隐藏掉
    function _closeAllDropDown(all_node_label,all_node_ul){
        document.onclick = function(event){
            for(var i = 0 ; i < all_node_label.length ;i++){
                if(event.target==all_node_ul[i]||event.target!=all_node_label[i]){
                    all_node_ul[i].style.display="none";
                }else{
                    all_node_ul[i].style.display="block";
                }
            }


        }
    }

    //开放出去
    CustomComponent.prototype.dropDown={
        init:_init
    };

})(CustomComponent)

//component.js
function Customponent(){}
var  custComponent=new Customponent();
```