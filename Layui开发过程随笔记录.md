## ==========开发随笔==========

- **表格顶部【删除已选】：$('.layui-btn[data-type="delSelected"]')**
- **分页的【确定】按钮：$(".layui-laypage-btn")**
- **机构弹出框选择的自定义行内操作按钮定义：**

    ```
        <script type="text/html" id="tableToolBar_tpl">
            <a class="layui-btn  layui-btn-xs" funName="{funName}" id="id_Choose" lay-event="InstitutionChoose">{btnName}</a>
        </script>
    ```

- **数据表格内是否显示操作按钮控制开关**

    - `config.isShowInLineOperationTool=true|false`

- **数据表格内是否显示数据表格行内复选框按钮开关**

    - `config.isShowTableCheckbox = true|false`
- **对于数据表配置的加载时隐藏的字段可在js逻辑进行设置后强制显示：`config.forceShowField=["insId"]`,将要强制加载显示的id放在一个数组中即可**

- **数据表格table选择**

    ```
        var checkStatus = table.checkStatus('tableID')
        var data = checkStatus.data;
    ```

- **数据表格列宽自适应出现横向滚动条的解决方案**
    1. 如果 table 是在 layui 的后台布局容器中（注意：不是在 iframe 中），在你的页面加上这段 CSS：

        ```
            .layui-body{overflow-y: scroll;}
        ```

    2. 如果 table 是在独立的页面，在你的页面加上这段 CSS：

        ```
            body{overflow-y: scroll;}
        ```
    > 总体而言，table 列宽自适应出现横向滚动条的几率一般是比较小的，主要原因是 table 的渲染有时会在浏览器纵向滚动条出现之前渲染完毕，这时 table 容器会被强制减少滚动条宽度的差（一般是 17px），导致 table 的横向滚动条出现。建议强制给你的页面显示出纵向滚动条。

- **switch开关必须放在form表单中，否则渲染失败，看不出效果**，例如：

    ```
    <form class="layui-form" action="">
        <div class="layui-form-item">
            <label class="layui-form-label">开关</label>
            <div class="layui-input-block">
                <input type="checkbox" name="close" lay-skin="switch" lay-text="是|否">
            </div>
        </div>
    </form>
    ```

- **switch开关监听**

    ```
        form.on('switch(filter)', function(data){
            console.log(data.elem); //得到checkbox原始DOM对象
            console.log(data.elem.checked); //开关是否开启，true或者false
            console.log(data.value); //开关value值，也可以通过data.elem.value得到
            console.log(data.othis); //得到美化后的DOM对象
        });  
    ```

    > 其中，filter需要自行定义lay-filter的值，例如：

    ```
        <div class="layui-form-item">
            <label class="layui-form-label">主键</label>
            <div class="layui-input-block">
                <input type="checkbox" name="close" lay-skin="switch" lay-filter="isPrimaryKey" lay-text="是|否">
            </div>
        </div>
    ```
- switch开关动态开关

    > 注意点：需要form.render()进行重新渲染，官方文档开头就说的很清楚。但是我想很多人有时都忽略了重新渲染，包括我自己 哈哈。


    - 重要的事说三遍:
        - **凡是涉及到form表单里面的内容，通过js动态处理完必须进行`form.render`渲染**
        - **凡是涉及到form表单里面的内容，通过js动态处理完必须进行`form.render`渲染**
        - **凡是涉及到form表单里面的内容，通过js动态处理完必须进行`form.render`渲染**
    ```
        // 设置选中状态
        $("#isPrimaryKey").attr('checked','checked');

        // 重新渲染，以便生效。此处为了方便直接render了，没有添加类型参数，各位可根据实际情况填写。
        form.render();
    ```

- **switch开关状态判断问题**

    > 问题背景说明：页面有一个switch开关，叫做“是否开启通知”，发送了一条请求查询一下这个开关是否开始，例如这么判断：

        if ($(switch开关选择器).val() == entity[inputName]){
            $(this).attr("checked", "checked");
            layui.use("form", function () {
                var form = layui.form;
                form.render();
            });
        }

    实际上entity[inputName]返回值为“true”，但是这个if条件不成立，导致出现超出自己预期的结果。如果你好好看文档你会发现，在 [http://www.layui.com/doc/element/form.html#switch](http://www.layui.com/doc/element/form.html#switch)  有这么一句描述：

    > 设置value="1"可自定义值，否则选中时返回的就是默认的on

    所以，实际的if判断是：if("on"=="true")，这显然是不成立的。因此需要改动一下你的html，加一个 `value`属性，例如：

    ```
    <input type="checkbox" id="isPrimaryKey" name="isPrimaryKey" value="true" lay-skin="switch" lay-filter="isPrimaryKey" lay-text="是|否">
    ```
        
- select下拉框通过设置 selected=""可以设置当前选择项，例如:

    ```
        <select>
            <option value="请选择表单类型..."></option>
            <option value="1" selected="">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
    ```
