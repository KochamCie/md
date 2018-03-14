> 思路

为了避免disconf文本模式或者自己上传配置值格式错误，当配置信息过多而json格式的信息不易于人为维护，寻得一款json-editor的控件，有很多功能易于编辑，容错性高





- ​
- ​
- 所需要的js及css

![json_editor_source](https://raw.githubusercontent.com/KochamCie/pic/master/jsoneditsource.jpg)



> 在页面中加入json-editor的div层
>
> - 在json-editor外整合合适效果的modal模态框，两者样式或脚本源码需要大调

```html
<!-- json editor 编辑层-->
<div class="je-container hide" id="je-container">
    <div class="row111">
        <div class="aw-content-wrap clearfix">
            <div class=" aw-main-content" style="min-height:700px;max-height:2000px">
                <div style="clear:both"></div>
                <div class="panel panel-default">
                    <style type="text/css">
                    </style>
                    <div class="panel-heading">
                        <div class="media">
                            <div class="media-body">
                            </div>
                        </div>
                    </div>
                    <div class="panel-body">
                        <div id="content-wrapper" style="height:600px">
                          <!-- 关键相关三个div为jsoneditor ，外层由modal控件包裹-->
                            <div id="jsonformatter" style="width: 48%; height: 512px;"></div>
                            <div id="splitter"></div>
                            <div id="jsoneditor" style="left: 597px; width: 48%;"></div>
                            <div class="clear"></div>
                            <input type="hidden" id="forButtonSave">
                        </div>
                    </div>
                    <div class="clear"></div>
                </div>
            </div>
        </div>
    </div>

</div>
```





>  需要简单判断配置值是否为json格式，

```javascript
// 判断是否是json格式，用于启动界面化配置
if (isJsonString(result.value)) {
    // 限制文本框的输入
    $("#value").attr("readonly", "readonly")
    $("#value").css("cursor","pointer")
    // 绑定事件
    $("#value").bind("click", function(e){
        // 清空相关控件的内容
        app.reload();
        var jsons = JSON.parse($(this).val());
        // render
        app.load(jsons);
        // app.resize，此方法中调整了获取控件长宽位置，用于子元素准确定位，不至于被遮盖或错位
        app.resize();
        // 弹出即全屏
        var index = layer.open({
            title:"编辑配置项:"+result.appName + "<b style='color:greenyellow'>*</b> " + result.version + " <b style='color:greenyellow'>*</b> "
            + result.envName,
            type: 1,
            content: $("#je-container"),
            area: ['320px', '195px'],
            maxmin: true,
            cancel: function () {
                if($("#forButtonSave").val()){
                    $("#value").val($("#forButtonSave").val())
                }
            }
        });
        layer.full(index);
    })
}

// isJsonString
function isJsonString(str) {
    try {
        if (typeof JSON.parse(str) == "object") {
            return true;
        }
    } catch(e) {
    }
    return false;
}
```

> 控件增加save功能

```javascript
// create save button 13点26分 2017年12月5日
    var buttonSave = document.createElement('button');
    //buttonCompact.innerHTML = 'Save';
    buttonSave.className = 'jsoneditor-menu jsoneditor-save';
    buttonSave.title = '保存编辑JSON信息';
    buttonSave.innerHTML = '保';
    this.menu.appendChild(buttonSave);




    buttonSave.onclick = function () {
        try {
            $("#forButtonSave").val("");
            var json = JSONEditor.parse(textarea.value);
            textarea.value = JSON.stringify(json);
            // 临时存入,由X保存替换 （点击关闭时存入）
            // 这个叉叉属于modal的，所以代码在上个代码块中的cancel部分
            $("#forButtonSave").val(textarea.value);
            layer.msg('已保存，请退出JSON eDiTOr页面');
        } catch (err) {
            me.onError(err);
        }
    };




```





> 该控件提供json格式化，json内容格式校验等功能。js部分调整完毕
>
> 接下来是关于样式方面调整，由于使用弹出全屏编辑，z-index过高。
>
> json-editor实现预分配的z-index一定几率低于弹层，导致一些je的类似hover显示select样式被遮盖
>
> 或者notify提示模块信息被遮挡。app.resize中不调整关于元素位置会造成hover显示的内容错位等等问题

```css
/** 给定弹出层样式 **/
.je-container {
    width: 1170px;
    padding-right: 15px;
    padding-left: 15px;
    margin-right: auto;
    margin-left: auto;
    background-color: #F8F8F8;
    min-height: 800px;
    z-index: 9999;
}

/** error提示信息，内定为最高值 **/
.error {
    z-index: 20171214;
}

/** 与error提示信息相同，内定为最高值 **/
div.jsoneditor-select {
	border: 1px solid gray;
	background-color: white;
	box-shadow: 4px 4px 10px rgba(128, 128, 128, 0.5);
	z-index: 20171214;
}


```

> 控件上增加手动保存功能，



> 还有页面零碎的调整，后续提到了再补充吧