# 01-canvas示例(旋转图片)



代码示例：

```vue
<template>
		<div class="rectify__img">
			<div>
				<img ref="rectifyImg" :src="src" alt="" />
			</div>

			<i @click="handleRotate" class="el-icon-refresh-right"></i>
		</div>
</template>
<script>
export default {
	components: {},
	props: {},
	data() {
		return {
			src: '',
			rotate: 0,
			file: null,
		};
	},
  created() {
    this.rotate = 0; // 初始化img标签角度
		this.$refs.rectifyImg.style.transform = `rotate(0deg)`;
  },
  methods: {
    		handleRotate() {
			this.rotate += 90;
			if (this.rotate >= 360) {
				this.rotate = 0;
			}
			const that = this;
			this.$refs.rectifyImg.style.transform = `rotate(${this.rotate}deg)`;

			const img = new Image(); // 创建一个新img元素
			img.src = this.src;
			img.setAttribute('crossOrigin', 'Anonymous'); // 防止跨域
			img.onload = function() {
				const canvas = document.createElement('canvas');
				const ctx = canvas.getContext('2d');
				canvas.width = img.width;
				canvas.height = img.height;
				if (that.rotate === 0) {
					ctx.drawImage(img, 0, 0);
				} else if (that.rotate === 90) {
					canvas.width = img.height;
					canvas.height = img.width;
					ctx.translate(canvas.width / 2, canvas.height / 2);
					ctx.rotate((that.rotate * Math.PI) / 180);
					ctx.drawImage(img, -canvas.height / 2, -canvas.width / 2);
				} else if (that.rotate === 180) {
					ctx.rotate((that.rotate * Math.PI) / 180);
					ctx.drawImage(img, -img.width, -img.height);
				} else {
					canvas.width = img.height;
					canvas.height = img.width;
					ctx.translate(canvas.width / 2, canvas.height / 2);
					ctx.rotate((that.rotate * Math.PI) / 180);
					ctx.drawImage(img, -canvas.height / 2, -canvas.width / 2);
				}
				const ndata = canvas.toDataURL('image/png', 1.0);
				that.file = that.dataURLtoFile(ndata);
				// console.log('文件', that.file);
			};
		},
		// base64转图片文件方法
		dataURLtoFile(dataurl) {
			var arr = dataurl.split(',');
			var bstr = atob(arr[1]);
			var n = bstr.length;
			var u8arr = new Uint8Array(n);
			while (n--) {
				u8arr[n] = bstr.charCodeAt(n);
			}
			return new File([u8arr], 'image', {
				type: 'image'
			});
		},
  }
}
</script>
```

注意：canvas拿到的文件格式是base64的，需要转换一下





## rotata方法

该方法也建议去MDN上查询

基础语法：

```js
void ctx.rotate(angle);
```



参数：

angle：顺时针旋转的**弧度**。如果你想通过角度值计算，可以使用公式： `degree * Math.PI / 180` 。





## drawImage方法

该方法可以去MDN上查

基础语法：

```js
drawImage(image, dx, dy);
drawImage(image, dx, dy, dWidth, dHeight);
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

