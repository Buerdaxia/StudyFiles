# MessageBox消息弹窗组件

## 使用：

一般都会将这个组件的作为一个方法挂到vue身上，到时候随时调用并使用

```
// 例如就使用MessageBox身上的confirm方法
import Vue from 'vue';
import {MessageBox} from 'element-ui';

// 挂在原型上，所有的组件实例都可以用
Vue.prototype.$confirm = MessageBox.confirm;
```



## 处理confirm方法

第一种用结构赋值+判断

```js
// 登出
    async loginout() {
      const result = await this.$confirm('确定要退出吗?', '退出提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).catch(err => err)
      if (result !== 'cancel') {
        this.$router.push({
          path: '/login'
        })
      }
      console.log(result);
    }
```



第二种：官网形式，用then，catch

```js
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
```

