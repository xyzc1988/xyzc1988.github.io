---
title: javaScript小例子
date: 2016-12-07 21:31:40
tags: javascript
---
## 搜索框效果
``` javascript
/** 
* @param inputName：文本框id 
* @param searchObj：搜索的UL的id 
*/
function searchName(inputName, searchObj) {    
    $("#" + inputName).on("input propertychange", function () {        
        var inputVal = $("#" + inputName).val();        
        if (inputVal != "") {            
            var searchObjLength = $("#" + searchObj + ">li").length;            
            $("#" + searchObj + ">li").addClass("hide");            
            for (var i = 0; i < searchObjLength; i++) {                
                if (($("#" + searchObj + ">li").eq(i).text().toLowerCase()).indexOf(inputVal.toLowerCase()) != -1) {    
                    $("#" + searchObj + ">li").eq(i).removeClass("hide");                
                }            
            }        
        } else {            
            $("#" + searchObj + ">li").removeClass("hide");        
        }    
    });
}﻿
```
**<!--more-->**
## 上移下移 
```javascript
$("#key_group_model_warp .table_td_text1_move_up_a").on("click",function() {
    var $tr = $(this).parent().parent();
    $tr.prev().before($tr);
    resetKeyGroupTrStyle();
});
$("#key_group_model_warp .table_td_text1_move_down_a").on("click",function() {
    var $tr = $(this).parent().parent("tr");
    $tr.next().after($tr);
    resetKeyGroupTrStyle();
});
function resetKeyGroupTrStyle() {
     var tr_count = $("#key_group_model_warp tr").length;
     $("#key_group_model_warp tr").find("a").css({"color": "#337ab7","pointer-events": "auto"});
     $("#key_group_model_warp tr").eq(0).find("a").eq(1).css({"color": "#ddd","pointer-events": "none"});
     $("#key_group_model_warp tr").eq(tr_count).find("a").eq(2).css({"color": "#ddd", "pointer-events": "none"});
}​﻿​
```

##截取文件名称
```javascript
function getUploadFileName(filepath){
    var pos;
    var regExp = /[a-zA-z]+:\\[^\s]*/; 
    if(regExp.test(filepath)){
        pos = filepath.lastIndexOf("\\");
    }else{
        pos = filepath.lastIndexOf("/");
    }
    return filepath.substring(pos + 1); 
}
```