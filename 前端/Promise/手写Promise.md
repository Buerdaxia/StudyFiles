# 手写Promise

整体代码：

```js
/**
 *
 * @param {Function} executor
 * @description 重写promise构造函数
 */
function Promise(executor) {
	// Promise实例对象自带的两个属性
	this.PromiseState = 'pending';
	this.PromiseResult = null;
	// 保存成功和失败的方法
	this.callbacks = [];
	// 保存当前this
	const self = this;
	/**
	 *
	 * @param {*} data
	 */
	function resolve(data) {
		// 判断一下PromiseState是否被改变了，如果变了直接返回
		if (self.PromiseState !== 'pending') return;
		// 由于resolve()为window调用，使用保存好的this
		// 1.修改对象状态（promiseState）
		self.PromiseState = 'fulfilled';
		// 2.设置对象结果（promiseResult）
		self.PromiseResult = data;

		// 调用成功的回调
		setTimeout(() => {
			self.callbacks.forEach(item => {
				item.onResolved(data);
			});
		});
	}
	/**
	 *
	 * @param {*} data
	 */
	function reject(data) {
		// 判断一下PromiseState是否被改变了，如果变了直接返回
		if (self.PromiseState !== 'pending') return;
		// 由于reject()为window调用，使用保存好的this
		// 1.修改对象状态（promiseState）
		self.PromiseState = 'rejected';
		// 2.设置对象结果（promiseResult）
		self.PromiseResult = data;

		// 执行失败的回调
		setTimeout(() => {
			self.callbacks.forEach(item => {
				item.onRejected(data);
			});
		});
	}

	// 处理throw抛出异常
	try {
		// 执行器函数：必须同步调用
		executor(resolve, reject);
	} catch (error) {
		// 修改Promise 对象状态为失败
		reject(error);
	}
}

// 原型身上添加then方法
Promise.prototype.then = function(onResolved, onRejected) {
	// 保存一下this
	const self = this;

	// 判断回调是否传递onRejected函数
	if (typeof onRejected !== 'function') {
		// 给onRejected赋值一个默认值
		onRejected = reason => {
			throw reason;
		};
	}

	// 判断回调是否传递onResolve方法
	if (typeof onResolved !== 'function') {
		onResolved = value => value;
	}

	// then方法无论如何都是返回一个promise对象，可能是失败的也可能是成功的
	return new Promise((resolve, reject) => {
		// 发现return中的4个try...catch非常一致，我们自己封装一下
		function callback(type) {
			try {
				/**
				 * @param 成功回调的值
				 */
				// 4个catch只有这里不一样
				let result = type(self.PromiseResult);

				if (result instanceof Promise) {
					// 返回的是一个promise对象时
					result.then(
						v => {
							// promise是成功那我也成功
							resolve(v);
						},
						r => {
							// promise是失败那我也失败
							reject(r);
						}
					);
				} else {
					// 返回的不是一个promise对象时
					resolve(result);
				}
			} catch (error) {
				// 捕获异常
				reject(error);
			}
		}

		// 调用回调函数
		if (this.PromiseState === 'fulfilled') {
			// try {
			// 	/**
			// 	 * @param 成功回调的值
			// 	 */
			// 	let result = onResolved(this.PromiseResult);

			// 	if (result instanceof Promise) {
			// 		// 返回的是一个promise对象时
			// 		result.then(
			// 			v => {
			// 				// promise是成功那我也成功
			// 				resolve(v);
			// 			},
			// 			r => {
			// 				// promise是失败那我也失败
			// 				reject(r);
			// 			}
			// 		);
			// 	} else {
			// 		// 返回的不是一个promise对象时
			// 		resolve(result);
			// 	}
			// } catch (error) {
			// 	// 捕获异常
			// 	reject(error);
			// }

			// 为了让 then的回调是异步的加一个方法
			setTimeout(() => {
				callback(onResolved);
			});
		}
		if (this.PromiseState === 'rejected') {
			// try {
			// 	/**
			// 	 * @param 失败回调的值
			// 	 */
			// 	let result = onRejected(this.PromiseResult);

			// 	if (result instanceof Promise) {
			// 		// 返回的是一个promise对象
			// 		result.then(
			// 			v => {
			// 				resolve(v);
			// 			},
			// 			r => {
			// 				reject(r);
			// 			}
			// 		);
			// 	} else {
			// 		// 返回的不是一个promise对象 都将promise改为成功
			// 		resolve(result);
			// 	}
			// } catch (error) {
			// 	// 捕获异常
			// 	reject(error);
			// }

			setTimeout(() => {
				callback(onRejected);
			});
		}

		// 判断 pending状态 异步时，promise状态不会即使改变，所以为pending
		if (this.PromiseState === 'pending') {
			// 思考？异步任务时什么时候去调用传入的onResolved和onRejected？
			// 答案：状态修改的时候进行调用，哪里进行状态修改呢？
			// .答案：在resolve()和reject()中

			// 保存一下，为了能让resolve()和reject()函数能够调用
			// 并切以数组方式存储，为了能让实例多个then方法去触发
			this.callbacks.push({
				onResolved: function() {
					// try {
					// 	let result = onResolved(self.PromiseResult);
					// 	if (result instanceof Promise) {
					// 		// 如果是一个promise对象
					// 		result.then(
					// 			v => {
					// 				// result是成功的promise 那我就调用resolve修改返回promise状态为成功
					// 				resolve(v);
					// 			},
					// 			r => {
					// 				// result是失败的就会走这个函数，那我也将返回promise状态修改为失败
					// 				reject(r);
					// 			}
					// 		);
					// 	} else {
					// 		// 如果不是一个promise对象
					// 		// 直接调用resolve方法，修改当前promise状态就行了
					// 		resolve(self.PromiseResult);
					// 	}
					// } catch (error) {
					// 	reject(error);
					// }
					callback(onResolved);
				},
				onRejected: function() {
					// try {
					// 	let result = onRejected(self.PromiseResult);
					// 	if (result instanceof Promise) {
					// 		// 如果是一个promise对象
					// 		result.then(
					// 			v => {
					// 				// result是成功的promise 那我就调用resolve修改返回promise状态为成功
					// 				resolve(v);
					// 			},
					// 			r => {
					// 				// result是失败的就会走这个函数，那我也将返回promise状态修改为失败
					// 				reject(r);
					// 			}
					// 		);
					// 	} else {
					// 		// 如果不是一个promise对象
					// 		// 直接调用resolve方法，修改当前promise状态就行了
					// 		resolve(self.PromiseResult);
					// 	}
					// } catch (error) {
					// 	reject(error);
					// }
					callback(onRejected);
				}
			});
		}
	});
};

// 原型上添加catch方法
Promise.prototype.catch = function(onRejected) {
	const self = this;
	return self.then(undefined, onRejected);
};

// 添加Promise的resolve方法
Promise.resolve = function(value) {
	if (value === 'undefined') {
		return new Promise(resolve => {
			resolve();
		});
	}
	// 返回结果是一个promise对象
	return new Promise((resolve, reject) => {
		if (value instanceof Promise) {
			value.then(
				v => {
					resolve(v);
				},
				e => {
					reject(e);
				}
			);
		} else {
			resolve(value);
		}
	});
};

// 添加Promise的reject方法
Promise.reject = function(reason) {
	return new Promise((resolve, reject) => {
		reject(reason);
	});
};

// 添加Promise的all方法
Promise.all = function(promises) {
	const result = [];
	//返回结果也是一个Promise对象
	return new Promise((resolve, reject) => {
		let count = 0;
		// 只要走完这个for循环都没有改变状态，promise状态必定会被外面的resolve改变
		for (let i = 0; i < promises.length; i++) {
			// 判断 数组中的promise对象是否成功
			promises[i].then(
				v => {
					result[i] = v;

					// push方法可能会导致数组顺序有问题
					// result.push(v);

					// 方法2
					// count++;
					// if (count === promises.length) {
					// 	// 如果表示为等于数组长度，则成功
					// 	resolve(result);
					// }
				},
				e => {
					reject(e);
				}
			);
		}

		// 如果for循环完毕都没有改变状态，那么三个都成功了
		resolve(result);
	});
};

// 添加Promise.race方法
Promise.race = function(promises) {
	return new Promise((resolve, reject) => {
		for (let i = 0; i < promises.length; i++) {
			/* 
        核心：如果状态改变，直接改变返回promise的状态即可
      */
			promises[i].then(
				v => {
					resolve(v);
				},
				e => {
					reject(e);
				}
			);
		}
	});
};

```



**class版本的**：

```js
class Promise {
	constructor(executor) {
		// Promise实例对象自带的两个属性
		this.PromiseState = 'pending';
		this.PromiseResult = null;
		// 保存成功和失败的方法
		this.callbacks = [];
		// 保存当前this
		const self = this;
		/**
		 *
		 * @param {*} data
		 */
		function resolve(data) {
			// 判断一下PromiseState是否被改变了，如果变了直接返回
			if (self.PromiseState !== 'pending') return;
			// 由于resolve()为window调用，使用保存好的this
			// 1.修改对象状态（promiseState）
			self.PromiseState = 'fulfilled';
			// 2.设置对象结果（promiseResult）
			self.PromiseResult = data;

			// 调用成功的回调
			setTimeout(() => {
				self.callbacks.forEach(item => {
					item.onResolved(data);
				});
			});
		}
		/**
		 *
		 * @param {*} data
		 */
		function reject(data) {
			// 判断一下PromiseState是否被改变了，如果变了直接返回
			if (self.PromiseState !== 'pending') return;
			// 由于reject()为window调用，使用保存好的this
			// 1.修改对象状态（promiseState）
			self.PromiseState = 'rejected';
			// 2.设置对象结果（promiseResult）
			self.PromiseResult = data;

			// 执行失败的回调
			setTimeout(() => {
				self.callbacks.forEach(item => {
					item.onRejected(data);
				});
			});
		}

		// 处理throw抛出异常
		try {
			// 执行器函数：必须同步调用
			executor(resolve, reject);
		} catch (error) {
			// 修改Promise 对象状态为失败
			reject(error);
		}
	}

	then(onResolved, onRejected) {
		// 保存一下this
		const self = this;

		// 判断回调是否传递onRejected函数
		if (typeof onRejected !== 'function') {
			// 给onRejected赋值一个默认值
			onRejected = reason => {
				throw reason;
			};
		}

		// 判断回调是否传递onResolve方法
		if (typeof onResolved !== 'function') {
			onResolved = value => value;
		}

		// then方法无论如何都是返回一个promise对象，可能是失败的也可能是成功的
		return new Promise((resolve, reject) => {
			// 发现return中的4个try...catch非常一致，我们自己封装一下
			function callback(type) {
				try {
					/**
					 * @param 成功回调的值
					 */
					// 4个catch只有这里不一样
					let result = type(self.PromiseResult);

					if (result instanceof Promise) {
						// 返回的是一个promise对象时
						result.then(
							v => {
								// promise是成功那我也成功
								resolve(v);
							},
							r => {
								// promise是失败那我也失败
								reject(r);
							}
						);
					} else {
						// 返回的不是一个promise对象时
						resolve(result);
					}
				} catch (error) {
					// 捕获异常
					reject(error);
				}
			}

			// 调用回调函数
			if (this.PromiseState === 'fulfilled') {
				setTimeout(() => {
					callback(onResolved);
				});
			}
			if (this.PromiseState === 'rejected') {
				setTimeout(() => {
					callback(onRejected);
				});
			}

			// 判断 pending状态 异步时，promise状态不会即使改变，所以为pending
			if (this.PromiseState === 'pending') {
				this.callbacks.push({
					onResolved: function() {
						callback(onResolved);
					},
					onRejected: function() {
						callback(onRejected);
					}
				});
			}
		});
	}

	catch(onRejected) {
		const self = this;
		return self.then(undefined, onRejected);
	}

	static resolve(value) {
		if (value === 'undefined') {
			return new Promise(resolve => {
				resolve();
			});
		}
		// 返回结果是一个promise对象
		return new Promise((resolve, reject) => {
			if (value instanceof Promise) {
				value.then(
					v => {
						resolve(v);
					},
					e => {
						reject(e);
					}
				);
			} else {
				resolve(value);
			}
		});
	}

	static reject(reason) {
		return new Promise((resolve, reject) => {
			reject(reason);
		});
	}

	static all(promises) {
		const result = [];
		//返回结果也是一个Promise对象
		return new Promise((resolve, reject) => {
			let count = 0;
			// 只要走完这个for循环都没有改变状态，promise状态必定会被外面的resolve改变
			for (let i = 0; i < promises.length; i++) {
				// 判断 数组中的promise对象是否成功
				promises[i].then(
					v => {
						result[i] = v;

						// push方法可能会导致数组顺序有问题
						// result.push(v);

						// 方法2
						// count++;
						// if (count === promises.length) {
						// 	// 如果表示为等于数组长度，则成功
						// 	resolve(result);
						// }
					},
					e => {
						reject(e);
					}
				);
			}

			// 如果for循环完毕都没有改变状态，那么三个都成功了
			resolve(result);
		});
	}

	static race(promises) {
		return new Promise((resolve, reject) => {
			for (let i = 0; i < promises.length; i++) {
				/* 
          核心：如果状态改变，直接改变返回promise的状态即可
        */
				promises[i].then(
					v => {
						resolve(v);
					},
					e => {
						reject(e);
					}
				);
			}
		});
	}
}

```

