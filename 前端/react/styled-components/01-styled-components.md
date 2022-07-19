# styled-components

作用：书写css像书写组件一样流畅，配合react组件可以快速创建高复用组件。

核心：给styled标签传递的属性，也会传递到props上，在css中通过`${(props) => {return xxx}}`访问

核心模板语法：

```js
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
  width: ${(props) => {return xxx}}
`;
```



## 基本使用

>安装：

```js
npm install --save styled-components

yarn add styled-components
```



>第二步：创建标签包裹App组件

```jsx
import { ThemeProvider } from 'styled-components';
import theme from './theme';// them是我们自己定义的主体文件
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<React.StrictMode>
    {/*这个theme全称贯穿styled，我们都能访问到这个属性*/}
		<ThemeProvider theme={theme}>
			<App />
		</ThemeProvider>
	</React.StrictMode>
);
```





>编写CSS样式：

```jsx
// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// Create a Wrapper component that'll render a <section> tag with some styles
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// Use Title and Wrapper like any other React component – except they're styled!
render(
  <Wrapper>
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```



## 编写一个Avatar组件

`components/Avatar/index.js`:

```jsx
import React from 'react';
import PropTypes from 'prop-types';
// 那这些styled组件替换原来的div等等的html标签
import { AvatarClip, AvatarImage, StatusIcon, StyledAvatar } from './style';

function Avatar({ src, size = '48px', status, statusIconSize = '8px', ...rest }) {
	return (
		<StyledAvatar {...rest}>
			{/* 如果status是空值，则不会显示 */}
			{status && <StatusIcon status={status} size={statusIconSize}></StatusIcon>}
			<AvatarClip size={size}>
				{/* 用styled替换了标签，但是仍然有html原来标签的属性 */}
				<AvatarImage src={src}></AvatarImage>
			</AvatarClip>
		</StyledAvatar>
	);
}

// 控制一下传入类型
Avatar.propTypes = {
	src: PropTypes.string.isRequired,
	size: PropTypes.string,
	status: PropTypes.oneOf(['online', 'offline']),
	statusIconSize: PropTypes.string
};

export default Avatar;

```



`components/Avatar/style.js`：

```jsx
import styled, { css } from 'styled-components';
import { CircleMixin } from 'utils/mixins';
// 写一个混入函数
const AvatarMixinFunc = (color, IconSize = '8px') => {
	return css`
		content: '';
		display: block;
		position: absolute;
		${CircleMixin(color, IconSize)}
	`;
};

const StyledAvatar = styled.div`
	position: relative;
`;

const StatusIcon = styled.div`
	position: absolute;
	/* styled也支持嵌套css语法和less一致 */
	&::before {
		${({ size }) => AvatarMixinFunc('#fff', size)};

		/* 这里scale就是很妙~的地方 */
		transform: scale(2);
	}

	&::after {
		${({ theme, status, size }) => {
			if (status === 'online') {
				return AvatarMixinFunc(theme.green, size);
			} else if (status === 'offline') {
				return AvatarMixinFunc(theme.gray, size);
			}
		}};
	}
`;

const AvatarClip = styled.div`
	/* 让头像蒙版取决于属性的宽高 */
	width: ${({ size }) => size};
	height: ${({ size }) => size};
	border-radius: 50%;
	overflow: hidden;
`;

const AvatarImage = styled.img`
	width: 100%;
	height: 100%;
	object-fit: cover;
	object-position: center;
`;

export { StyledAvatar, StatusIcon, AvatarClip, AvatarImage };

```





## 一些使用技巧

### 在普通HTML中也使用传递的props

我们可以通过css函数也能在普通标签中书写模板语法，来访问props，并且修改样式

>引入：

```
import 'styled-components/macro';
```

>使用:theme是我们之前定义好的主题，是一个对象

```jsx
		<div
			css={`
				background-color: ${({ theme }) => {
					return theme.darkPurple;
				}};
				width: 100px;
			`}
		>
			<MenuItem showBadge={true} active icon={faCommentDots}></MenuItem>
		</div>
```





### 如果出现嵌套关系，怎样修改css样式

例如：目前的`Avatar`组件在<StyledNavBar>标签包裹下，我们如何修改`Avatar`组件的css属性呢？

```jsx
function NavBar({ children, ...rest }) {
	return (
		<StyledNavBar {...rest}>
			<Avatar src={profileImg} status="online"></Avatar>
			<MenuItems>
				<MenuItem active showBadge icon={faCommentDots}></MenuItem>
				<MenuItem icon={faUsers}></MenuItem>
				<MenuItem icon={faFolder}></MenuItem>
				<MenuItem icon={faNoteSticky}></MenuItem>
				<MenuItem icon={faEllipsis}></MenuItem>
				<MenuItem icon={faGear}></MenuItem>
			</MenuItems>
		</StyledNavBar>
	);
}
```



解决方式：

首先我们先看一下，我们分装好的`Avatar`组件内容：

发现组件最外层标签为`StyledAvatar`包裹，所以我们就可以在styled模板语法中通过`${StyledAvatar}`来访问到整个`Avatar`组件并修改它的`css`属性

```jsx
function Avatar({ src, size = '48px', status, statusIconSize = '8px', ...rest }) {
	return (
		<StyledAvatar {...rest}>
			{/* 如果status是空值，则不会显示 */}
			{status && <StatusIcon status={status} size={statusIconSize}></StatusIcon>}
			<AvatarClip size={size}>
				{/* 用styled替换了标签，但是仍然有html原来标签的属性 */}
				<AvatarImage src={src}></AvatarImage>
			</AvatarClip>
		</StyledAvatar>
	);
}
```



修改步骤：`NavBar`组件的`style.js`

```js
import styled from 'styled-components';
// 第一步：引入Avatar的styled
import { StyledAvatar, StatusIcon } from '../Avatar/style';

const StyledNavBar = styled.nav`
	display: grid;
	grid-template-rows: 1fr 4fr;
	width: 100px;
	height: 100vh;
	background-color: ${({ theme }) => theme.darkPurple};
	padding: 30px 0;
	/*第二步直接通过${}访问并修改CSS样式*/
	${StyledAvatar} {
		justify-self: center;
		${StatusIcon} {
			&::before {
				background-color: ${({ theme }) => theme.darkPurple};
			}
		}
	}
`;

const MenuItems = styled.div``;

export default StyledNavBar;

export { StyledMenuItem, MenuIcon, MenuItems };

```

