# token(jwt)

## token

token本质上来说就是一段**加密过的字符串**，里面存储着一些用户的信息（如`id`,`username`等），在转换后可以将这些信息解析出来。

登录成功后，给用户颁发一个令牌`token`,用户在以后的每一次请求中都携带这个令牌

主要使用：`jwt`技术（json web token）

* header:头部
* payload:载荷
* signature:签名



## 安装

```
npm i jsonwebtoken
```

## 使用

jwt生成的token是放在返回数据中，所以一般放在`controller`层处理

改写`src/controller/user.controller.js`

注意：**关键代码在生成token哪里**

```js
// 导入jwt
const jwt = require('jsonwebtoken');
// 导入jwt加密
const { JWT_SECRET } = require('../config/config.default');
const { createUser, getUserInfo } = require('../service/user.service');

// 导入错误类型
const { userRegisterError } = require('../constant/err.type');
class UserController {
	async register(ctx, next) {
		// 1.获取数据
		// console.log(ctx.request.body);
		const { user_name, password } = ctx.request.body;

		// 2.操作数据
		/*
      将参数传递到service层去处理
      问题1：因为 createUser返回的是一个promise必须等到
      promise返回结果才能返回结果，所以使用await等待返回结果

      问题2：如果createUser失败了，await是不会的到错误结果的，
      需要使用一个try...catch
    */
		try {
			const res = await createUser(user_name, password);
			// 3.返回结果
			ctx.body = {
				code: 0,
				message: '用户注册成功',
				result: {
					id: res.id,
					user_name: res.user_name
				}
			};
		} catch (error) {
			console.error('用户注册失败', error);
			// 创建用户失败时，触发错误处理
			ctx.app.emit('error', userRegisterError, ctx);
		}
	}

	async login(ctx, next) {
		const { user_name } = ctx.request.body;
		/*
      在token的payload要记录，id，user_name，is_admin
    */
		// 1.获取用户信息
		try {
			// const res = await getUserInfo({ user_name });
			// // 从返回结果对象中剔除掉password,将其他的丢进userRest这个对象中
			// const { password, ...userRest } = res;
			// 上面两句简写
			const { password, ...res } = await getUserInfo({ user_name });
			ctx.body = {
				code: 0,
				message: '用户登录成功',
				result: {
					/*
						res:是要变为token的值，是一个对象，一般放id，user_name, is_admin这些信息
						JWT_SECRET:是一段加密字符随意写可以 后续验证时需要 就是jwt.verify()方法需要
						{ expiresIn: '1d' }:是token过期时间
					*/
					token: jwt.sign(res, JWT_SECRET, { expiresIn: '1d' })
				}
			};
		} catch (error) {
			console.error('用户登录失败', error);
		}
	}
}

// 导出这个类实例化的对象，这样可以调用register方法
module.exports = new UserController();

```



# 前端发送请求时处理token

当发起用户登录时，会获得一个`token`，是一串字符串。在之后的请求中，都会将这个`token`携带着来发送请求。来保证你就是当前的用户。



一般都会将这个`token`放到请求头中，在请求头的`Authorization`字段上（这个字段可以自己创建）

