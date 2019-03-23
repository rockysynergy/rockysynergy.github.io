---
layout: post
title: Layui在tp5里的使用技巧
tags: PHP, Database
---

使用layui开发后台管理前端界面的技巧汇总

## 传递资源的id
使用动态表格显示资源列表，用户可以点击查看或修改单个选定的资源，这个时候就需要传递选定资源的id给PHP。通常的方法是：
```JavaScript
	//监听工具条
	table.on('tool(table-release)', function(obj){
		var data = obj.data;
		switch(obj.event){
			case 'detail':
				detail({'get_json': 'detail', 'member_id': data.member_id}, "{:url('resource/detail')}?member_id="+data.member_id);
				break;
		}
  });
  
  	//查看明细
	function detail(aData, url){
		// 显示表单
		var view = layer.open({
			type: 2,
			title: '明细',
			content: url,
			area: ['90%', '90%'],
			btn: ['关闭'],
			yes: function(index, layero){
				layer.close(index);
				return false;
			}
		});
	}
```
在细节页面传递资源id到后端
```HTML
 <div class="layui-fluid">
 	<h2 id="member_id" style="display:none">{$member_id}</h2>
	<table id="table-release" lay-filter="table-release"></table>
</div>
```

```JavaScript
	var ad_table = table.render({
		elem: '#table-release',
		url: "{:url('resource/detail')}", 
		where: {
			'member_id': $('#member_id').text(),
			'get_json': 1
		},
		cols: [[
			{field: 'member_id', title: '会员ID', width: 80},
			{field: 'create_time_title', title: '时间', width: 80},
      		]],
		page: true,
		limit: 20,
		height: 'full-220',
		text: {none: '暂无相关数据'},
		autoSort: false,
		done:function(){
			
		}
	});
  ```
  
  ###后台处理代码
  ```PHP
      public function detail () {
        $member_id = $this->request->param('member_id');
        if ($this->request->param('get_json')) {
            $where = ['member_id', '=', $member_id];
            $list = ReleaseModel::getlist([$where], '*', 'create_time desc');            
            $this->ajax_return['data'] = $list['data'];
            $this->ajax_return['count'] = $list['count'];
            return json($this->ajax_return);
        } 
        $this->assign(['member_id' => $member_id]);    
        return $this->fetch('detail');
    }
```  
