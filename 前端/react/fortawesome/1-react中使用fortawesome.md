# react中使用fortawesome



## 安装

如果全部使用，可以安装这5个库：

```js
// fortawesome的react组件
yarn add @fortawesome/react-fontawesome 
// fontawesome的核心库
yarn add @fortawesome/fontawesome-svg-core 
// fontawesome的品牌库例如微信、微博
yarn add @fortawesome/free-brands-svg-icons 
// fontawesome的空心图标库
yarn add @fortawesome/free-regular-svg-icons 
// fontawesome的实心图标库
yarn add @fortawesome/free-solid-svg-icons
```



## 使用

### 图标的搜索

我们想要什么图标可以去官方的图标备忘表去搜：`https://fontawesome.com/search?s=solid%2Cbrands`

引入的时候，注意我们要把图标名称修改一下，搜到的名称是`fa-user`引入的是对象组件名称修改为`faUser`全部改成小驼峰命名法：

```
fa-comment-dots ==> faCommentDots
fa-user 				==> faUser
```





>第一步：引入组件和font图标

```js
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faCommentDots } from '@fortawesome/free-solid-svg-icons';
```



>第二步：编写

```jsx
<FontAwesomeIcon icon={faCommentDots}></FontAwesomeIcon>;
{/*icon是不同svg图标*/}
```



>第三步：修改大小颜色
>
>注意：fontawesome的图标颜色大小，是通过css属性直接修改的：color修改颜色，font-size修改大小

```jsx
	return (
		<div>
			<FontAwesomeIcon icon={faCommentDots} style={{ fontSize: '24px' }}></FontAwesomeIcon>
			<FontAwesomeIcon icon={faCommentDots} style={{ fontSize: '36px' }}></FontAwesomeIcon>
			<FontAwesomeIcon icon={faCommentDots} style={{ fontSize: '48px' }}></FontAwesomeIcon>
		</div>
	);
```

