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

例如：目前的`Avatar`组件在`<StyledNavBar>`标签包裹下，我们如何修改`Avatar`组件的css属性呢？

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
	/*第二步直接通过${}访问Avatar组件最外层标签并修改CSS样式*/
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



## 函数

### useTheme方法

调用后，返回一个你自定义的主题对象，让标签中也能使用主题变量

例如:

自定义`theme.js`文件：

```js
export default {
	primaryColor: 'red',
	green: '#34D859',
	gray: 'rgba(24, 28, 47, 0.2)',
	red: '#f34848',
	darkPurple: '#292f4c',

	// 输入框背景
	gray2: 'rgba(241, 237, 237, 0.3)',
	// placeholder文字颜色
	gray3: 'rgba(24, 28, 47, 0.3)',
	// 输入框文字
	grayDark: '#181c2f',

	// 字体
	normal: '1.4rem',
	medium: '1.6rem'
};

```

使用：`useTheme`方法

```jsx
import { useTheme } from 'styled-components';

function Search({ placeholder = '请输入内容...', ...rest }) {
  // 这里调用
	const theme = useTheme();
	return (
		<Input
			placeholder={placeholder}
      {/*使用theme.xxx访问自定义主题对象内容*/}
			prefix={<Icon icon={SearchIcon} color={theme.gray3} width={18} height={18} />}
			{...rest}
		></Input>
	);
```

### styled函数式写法

styled()函数式写法，可以直接使用你之前写好的styled组件的css样式。

styled函数式写法，根据传递的值不同，会有不同的效果。

**第一种：传递一个styled标签**

示例：

```js
import StyledText from 'components/Text/style';
const StyledParagraph = styled(StyledText)``;

// 这样的效果是，直接拥有所有StyledText的样式，并且可以在``中创建添加样式
```

**第二种：传递一个分装好的React组件**

示例：传递一个封装好的组件

```js
import Paragraph from 'components/Paragraph';
import styled from 'styled-components';
// 这样是传递了整个组件给Time，Time标签就是Paragraph组件
const Time = styled(Paragraph)``;
```



使用示例：

```js
// 这里StyledParagraph想拥有所有StyledText组件的css样式，并且自己的超出还隐藏
import StyledText from 'components/Text/style';
import styled from 'styled-components';

const StyledParagraph = styled(StyledText)`
	${({ ellipsis }) =>
		ellipsis &&
		css`
			text-overflow: ellipsis;
			white-space: nowrap;
			overflow: hidden;
		`}
`;

export default StyledParagraph;


```





### css函数

css函数支持我们书写一些css样式并赋值给一个变量

语法：

```js
import styled, { css } from 'styled-components';
const demo = css`
	font-size: 14px;
`
```



我们一般使用他来做一些样式的变体：

```js
// 创建一个对象，+css函数，访问时通过键名儿访问
import styled, { css } from 'styled-components';

// 字体类型变体
const typeVariants = {
	primary: css`
		color: ${({ theme }) => theme.grayDark};
	`,
	secondary: css`
		color: ${({ theme }) => theme.grayDark};
		opacity: 0.3;
	`,
	danger: css`
		color: ${({ theme }) => theme.red};
	`
};

// 字体符号变体
const sizeVariants = {
	normal: css`
		${({ theme }) => theme.normal}
	`,
	medium: css`
		${({ theme }) => theme.medium}
	`,
	large: css`
		${({ theme }) => theme.large}
	`,
	xlarge: css`
		${({ theme }) => theme.xlarge}
	`,
	xxlarge: css`
		${({ theme }) => theme.xxlarge}
	`,
	small: css`
		${({ theme }) => theme.small}
	`,
	xsmall: css`
		${({ theme }) => theme.xsmall}
	`,
	xxsmall: css`
		${({ theme }) => theme.xxsmall}
	`
};

const StyledText = styled.span`
	font-size: ${({ size }) => sizeVariants[size] || sizeVariants.normal};
	${({ bold }) => bold && 'font-weight: 500'};
	${({ type }) => typeVariants[type]};
`;

export default StyledText;

```



### attrs函数

`attrs`函数，允许我们给一个`styled`标签用对象的方式传递属性和属性值。

示例：

```js
import styled from 'styled-components';
// 这里我们引入封装好的Text
import Text from 'components/text';
// 创建一个Name标签，应用Text组件，同时还传递了一个size属性
const Name = styled(Text).attrs({ size: 'large' });
const StyledMessageCard = styled.div``;

export default StyledMessageCard;

```



## as属性

as属性可以指明一个styled标签是HTML中的一种元素。

示例：

```jsx
{/*这里就是 styledParagraph就被渲染为p标签*/}
<StyledParagraph as="p" ellipsis={ellipsis} {...rest}>
  {children}
</StyledParagraph>
```



## ${}的作用

${}的作用，我使用的有两种：

1. 访问变量，访问styled标签传递的属性值
2. 访问定义好的styled标签，来修改整体css样式



示例1：访问变量

```react
// 页面结构，index.js
<StyledChatBubble type={type} {...rest}>
</StyledChatBubble>

// 样式style.js
const StyledChatBubble = styled.div`
	display: flex;
	flex-direction: column;
	/*这里解构属性中的type*/
	${({ type }) => typeVariants[type]}
`;
```



示例2：功能2访问定义好的`style`标签

```js
import styled, { css } from 'styled-components';
import Icon from 'components/Icon';
import Paragraph from 'components/Paragraph';
import Text from 'components/Text';

const Time = styled(Paragraph).attrs({ type: 'secondary', size: 'small' })`
	margin: 6px;
	margin-left: 24px;
	word-spacing: 1rem;
`;

const BubbleTip = styled(Icon)`
	position: absolute;
	bottom: -15%;
	left: 0;
	z-index: 5;
`;

const Bubble = styled.div`
	padding: 15px 22px;
	box-shadow: 0px 8px 24px rgba(0, 0, 0, 0.1);
	border-radius: 100px;
	position: relative;
	z-index: 10;
`;

const MessageText = styled(Text)``;

const StyledChatBubble = styled.div`
	display: flex;
	flex-direction: column;
	${({ type }) => typeVariants[type]}
`;

// typeVariants是放在StyledChatBubble下,所以可以访问她下面所有styled标签
const typeVariants = {
	mine: css`
		/*这里访问到Bubble标签所有属性*/
		${Bubble} {
			background-color: ${({ theme }) => theme.primaryColor};
		}

		${BubbleTip} {
			transform: rotateY('180deg');
			/* unset是取消原来left的值 */
			left: unset;
			right: 0;
		}

		/* svg中的path来控制svg的颜色的 */
		path {
			fill: ${({ theme }) => theme.primaryColor};
		}

		${Time} {
			text-align: right;
			margin-left: 0;
			margin-right: 24px;
		}

		${MessageText} {
			color: white;
		}
	`
};

export default StyledChatBubble;
export { MessageText, Bubble, BubbleTip, Time };

```

