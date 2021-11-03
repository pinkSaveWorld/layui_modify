# layui 改动

  

## tree 组件支持懒加载：options对象里面需要添加一个异步函数，键名为lazyLoad

```javascript
tree.render({
        lazyLoad: function (id) {
        	// console.log([1,2,3])
        	return new Promise(function(res, rej){
				if (!asyncData[id]) {
					alert('获取失败')
					rej('errorMsg') // 异步失败传一个错误信息
				} else {
					setTimeout(function () {
						res(asyncData[id]) // 成功的话将会返回的数组传过去
					}, 20)
				}
        	})
        }
    })
```



## tree 组件的复选框支持父子不联动，点一个选一个，options对象里面需要加一个布尔值，键值为oneByone

```javascript
tree.render({
    oneByOne: true, // 为true则表示取消父子节点复选框的联动，实现点一个选一个
})
```

## switch 开关组件支持异步操作改变状态，元素上要加async标签，监听返回的data对象中返回了一个dowhat函数，函数接收一个布尔值参数，为true的时候开开关，为false的时候关开关

```javascript
// html元素
<input type="checkbox" name="switch" lay-skin="switch" lay-text="on|off" lay-filter="fl" async>

// JS代码
layui.use('form', function(){
	var form = layui.form
	form.on('switch(fl)', function(data){
		console.log(data)
		data.dowhat(true)
        // data.dowhat(false)
	})
})
```

## table支持最大高度参数maxHeight，（height存在的话以height 为主），使用maxHeight 会导致table的render函数执行两遍，感觉性能会有所浪费

```javascript
table.render({
		maxHeight: 900,
    });
```

## select 支持多选下拉框，select标签上要加上 multi 属性

```html
<select name="roleDescriptions" multi><select>
// 返回的value 值 "1,2" 以逗号隔开的字符串
```

## select支持异步搜索功能，select标签上加上 lay-remote-search 属性和自定义的lay-filter，然后通过form.on 监听

```html
<div class="layui-form" style="width: 400px" lay-filter="test">
	<div class="layui-form-item">
        <label class="layui-form-label">员工名称</label>
        <div class="layui-input-block">
            <select name="workName" lay-remote-search lay-filter="search_test" id="workName"></select>
        </div>
    </div>
</div>
```

```javascript
layui.use('form', function(){
	form = layui.form
	form.on('remoteSearch(search_test)', function(data){
		document.getElementById('workName').innerHTML = `<option></option>`
        form.render('select', 'test', '#workName')
        // 根据搜索到的数据 可以动态生成 option 元素 插入到select 元素中，再render
	});
})
```

