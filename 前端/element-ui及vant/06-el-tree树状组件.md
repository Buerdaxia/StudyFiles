# 06-el-tree(父子节点同时选中)

当我们使用树状组件，做权限管理相关内容时，我们想要选中子节点，**一并将父节点及子节点的所有id全部保存下来**



```vue
<template>
  <el-tree
    ref="tree"
    @check-change="handleCheckChange"
    :data="treeData"
    show-checkbox
    node-key="id"
    :props="defaultProps"
    default-expand-all
  >
  </el-tree>
</template>

<script>
export default {
  name: 'tree',
  data() {
    return {
      dataForm: {
        rscoIdList:[]
      }
    }
  }
  methods: {
    handleCheckChange() {
			// 获取选中的子节点
			let leafNodeKeys = this.$refs['tree'].getCheckedKeys();
			// 获取选中的父节点（所有半选节点的id数组）
			let hafCheckedKeys = this.$refs['tree'].getHalfCheckedKeys();
			// 合并
			this.dataForm.rscoIdList = leafNodeKeys.concat(hafCheckedKeys);
		}
  }
}
</script>
```



**注意：当通过详情获取到ids后，对树状列表进行反选时，一般我们会使用这个API（`setCheckedKeys(keys)`）**

这个`api`的功能可以去`element-ui`官网去查看

该`api`有个小特点，他是只需要传递叶子节点的id就能够将该叶子节点的父节点以及祖先节点一并选中，然鹅，后端往往会在详情接口中返回全部选中的`id`(包括叶子节点、父节点)，所以我们需要自己判断一下，判断的代码如下：

```vue
<script>
export default {
  methods: {
      getTree() {
        let url = `/breeding/role/menuTreeList`;
        if (this.dataForm.id) {
          url = `/breeding/role/menuTreeList?subId=${this.dataForm.id}`;
        }
        this.$http.get(url).then(({data: res}) => {
          this.treeData = res.data.list;

          // 如果选中列表有数据，则复制给选中的角色id数组(编辑时会触发)
          // 注意这里：详情时checkedKeys是所有选中的id
          if (res.data.checkedKeys && res.data.checkedKeys.length > 0) {
            this.$nextTick(() => {
              let keys = [];
              res.data.checkedKeys.forEach(item => {
                // 根据id 拿到 Tree 组件中的node的所有信息
                let node = this.$refs['tree'].getNode(item);
                // node.isLeaf：判断当前节点是否为子节点
                if (node && node.isLeaf) {
                  //如果是子节点，就记录一下，后续要选中
                  keys.push(item);
                }
              });
              // 设置一下选中的树结构(有子节点id就可以了，不需要父级id，否则会重复)
              this.$refs['tree'].setCheckedKeys(keys);
            });
          }
      });
  	}
  }
}
</script>
```

