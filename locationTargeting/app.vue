<template>
<div class='tree'>
<div class='col-sm-6' >
	<el-tree
	:data="treeData"
	show-checkbox
	node-key="id"
	id='1'
	:default-expanded-keys="[]"
	:default-checked-keys="selectArr"
	:props="defaultProps"
	@check-change="getCheckedNodes"
	ref="tree"
	class='treeHeight'
	>
	</el-tree>
	<div class='col-sm-12' style='border: 1px solid #d1dbe5 ;padding:10px;'>
		<el-button @click="selectAll" type="text" class="btn pull-right" >全部</el-button>
	</div>
</div>
<div class='col-sm-6'>
<el-tree
  :data="data1"
  node-key="id"
  id='2'
  :default-expanded-keys="[]"
  :default-checked-keys="selectArr"
  :props="defaultProps"
   ref="selectedTree"
  :render-content="renderContent"
  class='treeHeight'
  >
</el-tree>
<div class='col-sm-12' style='border: 1px solid #d1dbe5 ;padding:10px;'>
		<el-button  @click="clear" type="text" class="btn pull-right" >清除</el-button>
</div>

</div>
</div>
</template>

<script>
var timeout;
import Vue from 'vue';
import {Tree,Button} from 'element-ui';
Vue.use(Tree);
Vue.use(Button);
	export default {
		props: ['treeData'],
		data:function(){
		return {
			selectArr:[],
			selectObj:{},
			arrds:[],
				defaultProps: {
				children: 'children',
				label: 'label'
				}
			}
		},
		created:function(){
			this.data1	=JSON.parse(JSON.stringify(this.treeData));
		},
		mounted:function(){
		},
		methods:{
			getSelectById(){
				return this.$refs.tree.getCheckedKeys();
			},
			getSelectByNode(){
				return this.$refs.tree.setCheckedNodes();
			},
			setSelectById(val){
				return this.$refs.tree.setCheckedKeys(val);
			},
			//  选择节点
			 getCheckedNodes( node, data, store ) {
				 var vm=this;
				 if(timeout){
					 clearTimeout(timeout);  
				 }
				timeout = setTimeout(function() {
					vm.selectArr=vm.$refs.tree.getCheckedNodes();
					var selected = vm.$refs.tree.root.childNodes;
					var show = vm.$refs.selectedTree.root.childNodes;
					vm.renderShow(selected,show);
				 }, 100);
			},
			clear:function(){
				this.$refs.tree.setCheckedKeys([]);
			},
			selectAll:function(){
				var nodes=this.$refs.tree.root.childNodes;
				var arr=[];
				for(let i =0 ;i<nodes.length;i++){
					arr.push(nodes[i].data.id);
				}
				this.$refs.tree.setCheckedKeys(arr);
			},
			//返回选中的节点，不返回父节点 暂时弃用
			fileterChildrenNode:function(data){
				let node = data
				let arr=[];
				while (node.length) {
					let item = node.pop();
					let ok = false
					for(let i = node.length-1;i>=0;i--){
						let searchItem = node[i];
						if(searchItem.children && searchItem.children.length > 0){
							let children=searchItem.children;
							if(children.indexOf(item)>=0){
								ok=true
							}

						}
					}
					if(!ok)
						{
							arr.push(item);
						}
				}
				return arr
			},
			// 渲染选中对象的树
			renderShow(data,showNode){
						for(let i =0 ;i<showNode.length;i++){
							var node = data[i];
							var show = showNode[i];
							if(!node.checked && !node.indeterminate){
								show.visible=false;
							}else if(!node.checked&&node.indeterminate){
								show.visible=true;
								if(node.childNodes && node.childNodes.length > 0){
									this.renderShow(node.childNodes,show.childNodes);
								}
							}
							else if(node.checked ){
								show.visible=true;
								if(node.childNodes && node.childNodes.length > 0){
									this.renderShow(node.childNodes,show.childNodes);
								}
							}
						}
				return showNode;
			},
			renderContent(h, { node, data, store }) {
				node.visible=false;//默认不显示选中的树数据
				return (
				<span>
					<span>
					<span>{node.label}</span>
					</span>
					<span style="float: right; margin-right: 20px">
					<el-button size="mini" on-click={ () => this.remove(store, data) }>Delete</el-button>
					</span>
				</span>);
			},
			 remove(store, data) {
				this.$refs.tree.setChecked(data,false);
			},
		},
		watch:{
		}
	}
</script>

<style lang='scss' scoped>
@import url("//unpkg.com/element-ui@1.4.1/lib/theme-default/index.css");
.tree{
	.treeHeight{
		height:200px; 
		overflow-y: auto;
	}
}

</style>