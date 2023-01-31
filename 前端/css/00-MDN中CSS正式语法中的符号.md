# MDN中CSS正式语法中的符号



这些是出现的符号含义：

具体请查看：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Value_definition_syntax#double_bar

| 符号     | 名称         | 描述                                 | 示例                                                |
| :------- | :----------- | :----------------------------------- | :-------------------------------------------------- |
| 组合符号 |              |                                      |                                                     |
|          | 并置         | 各部分必须出现且按顺序出现           | `solid <length>`                                    |
| `&&`     | “与”组合符   | 各部分必须出现，但可以不按顺序       | `<length> && <string>`                              |
| `||`     | “或”组合符   | 各部分至少出现一个，可以不按顺序     | `<'border-image-outset'> || <'border-image-slice'>` |
| `|`      | “互斥”组合符 | 各部分恰好出现一个                   | `smaller | small | normal | big | bigger`           |
| `[ ]`    | 方括号       | 强调优先级                           | `bold [ thin && <length> ]`                         |
| 数量符号 |              |                                      |                                                     |
|          | 无数量符号   | 恰好一次                             | `solid`                                             |
| `*`      | 星号         | 零次、一次或多次                     | `bold smaller*`                                     |
| `+`      | 加号         | 一次或多次                           | `bold smaller+`                                     |
| `?`      | 问号         | 零次或一次（即可选）                 | `bold smaller?`                                     |
| `{A,B}`  | 大括号       | 至少`A`次，至多`B`次                 | `bold smaller{1,3}`                                 |
| `#`      | 井号         | 一次或多次，但多次出现必须以逗号分隔 | `bold smaller#`                                     |
| `!`      | 叹号         | 组必须产生一个值                     | `[ bold? smaller? ]!`                               |