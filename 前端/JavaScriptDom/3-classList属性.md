# classList属性

## 定义和用法

classList 属性返回元素的类名，作为 DOMTokenList 对象。

该属性用于在元素中添加，移除及切换 CSS 类。

classList 属性是只读的，但你可以使用 add() 和 remove() 方法修改它。

## 用法

| 方法                          | 描述                                                         |
| :---------------------------- | :----------------------------------------------------------- |
| add(*class1, class2, ...*)    | 在元素中添加一个或多个类名。  如果指定的类名已存在，则不会添加 |
| contains(*class*)             | 返回布尔值，判断指定的类名是否存在。可能值：true - 元素包已经包含了该类名false - 元素中不存在该类名 |
| item(*index*)                 | 返回元素中索引值对应的类名。索引值从 0 开始。  如果索引值在区间范围外则返回 *null* |
| remove(*class1, class2, ...*) | 移除元素中一个或多个类名。  **注意：** 移除不存在的类名，不会报错。 |
| toggle(*class,* true\|false)  | 在元素中切换类名。  第一个参数为要在元素中移除的类名，并返回 false。 如果该类名不存在则会在元素中添加类名，并返回 true。  第二个是可选参数，是个布尔值用于设置元素是否强制添加或移除类，不管该类名是否存在。例如：  移除一个 class: *element*.classList.toggle("classToRemove", false); 添加一个 class: *element*.classList.toggle("classToAdd", true);  **注意：** Internet Explorer 或 Opera 12 及其更早版本不支持第二个参数。 |
