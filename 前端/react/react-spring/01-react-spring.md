# react-spring

react-spring是官方推荐的动画库之一。

react-spring和其他动画库不相同的是，它模拟的是弹簧的工作原理。三个属性：mass（质量）、tension（张力）、friction（摩擦力）

质量越大，运动越慢，张力越大弹出的越快，摩擦力越小回弹的次数越多。



## 标签`<animated.div>`

执行动画，需要给要执行的元素(标签)，外层套上一个`<animated.div></<animated.div>`标签，特别像styled-components一样。

然后给这个标签的**style属性**设置相应的动画。

## hooks

这些是react-spring自带的一些hooks函数，方便使用。

### useSpring

将一个数值过度到另一个数值。

设置一个单一执行的动画效果（可以说是设置一个动画，或者设置一个弹簧）



### useSprings

是设置多个弹簧（动画），每一个弹簧中的属性从一个数值过渡到另一个数值。



### useTrail(列表动画)

用于对同一组组件设置，设置多个弹簧，通过时间间隔相继进行执行，实现一种堆叠的效果。

例如：列表从上到下一个一个接着进入的效果。

语法：

```js
import {useTrail} from 'react-spring';

const trailAnimes = useTrail(参数一，参数二)
```

参数一：列表中的项的数量

参数二：动画的配置

返回结果：`trailAnimes`是一个数组，值是相同索引列表的执行动画，后续要绑定到`<animate.div>`的style属性上。

示例：(会有一个从左到右的动画效果)

```react
import { useTrail, animated } from 'react-spring';

function MessageList({ children, ...rest }) {
	const trailAnimes = useTrail(6, {
		// 1.确定对那个属性配置动画(translate3d能开启gpu加速，对性能较好，所以不用tanslateX)
		transform: 'translate3d(0px, 0px, 0px)',
		// 2.设置初始值从-50px开始移动
		from: { transform: 'translate3d(-50px, 0px, 0px)' },
		// 弹簧配置
		config: {
			mass: 0.8,
			tension: 280,
			friction: 20
		},
		// 3.延迟，每隔200ms下一个执行
		delay: 200
	});

	return (
		<StyledMessageList {...rest}>
			<FilterList options={['最新消息优先', '好友列表优先']} actionLabel="创建会话">
				<ChatList>
					{[1, 2, 3, 4, 5, 6].map((_, index) => {
						return (
							<animated.div key={index} style={trailAnimes[index]}>
								<MessageCard
									key={index}
									replied={index % 2 === 0}
									active={index === 3}
									avatarSrc={faceUrl1}
									avatarStatus="online"
									statusText="在线"
									name="钱不二"
									time="2 小时钱"
									message="不妨活的大胆一些,面试冲冲冲"
									unreadCount={2}
								></MessageCard>
							</animated.div>
						);
					})}
				</ChatList>
			</FilterList>
		</StyledMessageList>
	);
}
```





### useTransition(路由过度)

用于多个组件之间的切换过度效果，例如一个组件的创建，移除等





### useChain

用于顺序执行动画，前一个执行完毕，再执行下一个。