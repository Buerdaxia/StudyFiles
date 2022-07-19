# TS中的新东西interfaces

作用：利用`interface`可以创建出一种新类型(`type`，使用就和基础类型一样)，这种新类型用来描述对象的属性名和值的类型



简单来说：`object`类型的类型注释，如果对象属性太多，就会导致类型注释非常非常长而且难看，利用`interface`可以统一的处理属性类型，进而简化



简单使用：

```tsx
interface Vehicle {
	name: string;
	year: number;
	broken: boolean;
}

const oldCivic = {
	name: 'civic',
	year: 2000,
	broken: true
};

// 这里的object类型注释太长了，及不好看又多
// const printVehicle = (vehicle: { name: string; year: number; broken: boolean }): void => {
// 	console.log(`Name: ${vehicle.name}`);
// 	console.log(`Name: ${vehicle.year}`);
// 	console.log(`Name: ${vehicle.broken}`);
// };

// 我们可以创建一个interface来替代
const printVehicle = (vehicle: Vehicle): void => {
	console.log(`Name: ${vehicle.name}`);
	console.log(`Name: ${vehicle.year}`);
	console.log(`Name: ${vehicle.broken}`);
};
printVehicle(oldCivic);

```



## 高级使用(可重用的interface)

interface接口的检查顺序是，只要对象满足了我接口中的东西即可，其他无所谓。

意思就是我接口中定义的东西，你对象中存在，那么就不会报错



作用：这样可以在两个不同的对象中，公用一个`interface`，`TS`提倡我们这样编写不论是编写函数还是编写代码，还是编写`JS`，都应当遵循这种编写模式



示例：

```tsx
interface Vehicle {
	name: string;
	year: Date;
	broken: boolean;
	// 定义个函数，函数返回string类型
	summary(): string;
}

// 可重用的接口
interface Reportable {
	summary(): string;
}

const oldCivic = {
	name: 'civic',
	year: new Date(),
	broken: true,
	summary(): string {
		return `Name: ${this.name}`;
	}
};

const beverage = {
	color: 'brown',
	carbonate: true,
	sugar: 40,
	summary(): string {
		return `Name: ${this.color}`;
	}
};

// 这里的object类型注释太长了，及不好看又多
// const printVehicle = (vehicle: { name: string; year: number; broken: boolean }): void => {
// 	console.log(`Name: ${vehicle.name}`);
// 	console.log(`Name: ${vehicle.year}`);
// 	console.log(`Name: ${vehicle.broken}`);
// };
// 我们可以创建一个interface来替代
// const printVehicle = (vehicle: Vehicle): void => {
// 	console.log(vehicle.summary());
// };

const printSummary = (item: Reportable): void => {
	console.log(item.summary());
};
// 同一个函数不同作用
printSummary(oldCivic);
printSummary(beverage);
```





## 可重用代码的编写方式

在`TS`中编写可重用代码的策略

1. 在创建接收输入参数的函数时，要使用接口
2. 对象或者类能够给定一个满足接口的函数

![03-interfaces2](../../前端图片/typescript/03-interfaces2.PNG)



## `implements`关键字(不是必须要写的，但是有帮助)

在`class`和`interface`共同使用时，`TS`报错信息有时会不完整，就是并没有指明错误地点，我们可以借助`implements`关键字来帮助我们，确保这个类(`class`)，满足我们定义的接口(`interface`)

作用：**如果一个类使用了`implements`关键字，该类就必须要满足接口的所有属性**



示例：

```tsx
// 我们声明了一个接口，接口中的color属性，User类和Company类都没有
export interface Mappable {
	location: {
		lat: number;
		lng: number;
	};
	markerContent(): string;
	color: string;
}
```

报错信息：

`TS`只帮我们找到了这两处有问题，但是没有指出出问题的文件是哪一个

![7-implements关键字](../../前端图片/typescript/7-implements关键字.PNG)



使用`implements`关键字

示例：

```tsx
import faker from 'faker';
// 引入接口
import { Mappable } from './CustomMap';
// 使用implements关键字
export class User implements Mappable {
	name: string;
	location: {
		lat: number;
		lng: number;
	};
	constructor() {
		this.name = faker.name.firstName();
		// 给location初始化
		this.location = {
			// parseFloat()将字符串转换为number
			lat: parseFloat(faker.address.latitude()),
			lng: parseFloat(faker.address.longitude())
		};
	}

	markerContent(): string {
		return `User Name: ${this.name}`;
	}
}

```



注意报错：

使用关键字后，`TS`不仅帮我们提示报错信息，并且还帮助我们直接将有错误的文件标出来了

![8-implements关键字2](../../前端图片/typescript/8-implements关键字2.PNG)