# yarn安装与使用

## 安装

可以通过`npm`来安装`yarn`

```
npm install -g yarn
// 全局安装 yarn
```



## 常用命令

| npm                                                | yarn                     |
| -------------------------------------------------- | ------------------------ |
| npm init -y                                        | yarn init -y             |
| npm install react --save（--save可以不写默认就有） | yarn add react           |
| npm uninstall react --save                         | yarn  remove react       |
| npm install react --save -dev                      | yarn add react --dev     |
| npm update --save                                  | yarn upgrade             |
| npm install -g @vue/cli                            | yarn global add @vue/cli |

**npm全局安装是加-g**

**yarn全局安装时加global**

yarn add 包名：用yarn安装指定包



## 解决yarn全局安装后，命令不生效

出现提示xxx是外部命令等等...

解决办法：

1. 执行如下命令，得出yarn全局安装的命令所处的安装目录

   ```
   yarn global bin
   ```

2. 然后将得到的安装目录放到`path`

   放到环境变量的中的系统变量`path`中

![解决yarn环境变量问题](../../../../笔记/StudyFiles/前端图片/npm与yarn/解决yarn环境变量问题.png)

**如果还不生效，点击右边的上移将这行移动到最上面**