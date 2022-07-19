# GitHub搜索案例总结

1. 设计状态时要考虑全面，例如：带有网络请求的组件，要考虑失败怎么办？要考虑加载的时候怎么办？

2. ES6小知识点：解构赋值+重命名

3. 消息订阅与发布机制

   1. 先订阅，再发布
   2. 适用于任何组件间通讯
   3. 要再组件中的`componentWillUnmount`中取消订阅

4. fetch发送请求（关注分离的设计思想）就是发送请求分成了2步，第一步联系服s务器，第二步再获取数据

   ```js
   		try {
   			const response = await fetch(`http://localhost:3000/api/search/users2?q=${keyWord}`);
   			const data = await response.json();
   			PubSub.publish('updateListState', {
   				isFirst: false,
   				isLoading: false,
   				users: data.items
   			});
   			console.log('获取数据成功', data);
   		} catch (error) {
   			PubSub.publish('updateListState', { isFirst: false, isLoading: false, err: error });
   		}
   ```

   