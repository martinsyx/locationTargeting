# 地域定向

根据选择的城市 进行定向

  - 从左侧选择需要定向测城市
  - 右侧树中显示选中的定向城市，并且可取消定向


### 使用指南

导入组件

```sh
import locationTargeting from 'component/locationTargeting/app.vue';

 var app = new Vue({
.......
components: {
			"location":locationTargeting,
		}
})
```

在页面中使用组件
```sh
<location v-bind:tree-data="treeData" ref='tree'></location>
```

获得选择的城市id列表
```sh
var data=this.$refs.tree.getSelectById();
```

### Plugins
| name | README |
| ------ | ------ |
| :tree-data = 'data' | 给组件传入选择值，需要是一个Json， |
| ref="tree" | 对于组件的引用，以获取组件对象 |
| getSelectById()| 获得选择的定向城市id列表 |



 
模拟数据
```sh
var data=[
				{
				id: 1,
				label: '一级 1',
				children: [{
					id: 4,
					label: '二级 1-1',
					children: [{
					id: 9,
					label: '三级 1-1-1'
					}, {
					id: 10,
					label: '三级 1-1-2'
					}]
				}]
				}, 
				{
				id: 2,
				label: '一级 2',
				children: [{
					id: 5,
					label: '二级 2-1'
				}, {
					id: 6,
					label: '二级 2-2'
				}]
				}, {
				id: 3,
				label: '一级 3',
				children: [{
					id: 7,
					label: '二级 3-1'
				}, {
					id: 8,
					label: '二级 3-2'
				}]
				}]
```