# `html`表单相关

首先`html`表单提交，是前端提交数据的主要方式之一。

首先看一下，表单相关的几个最核心的标签：

## form标签

`MDN`解释：**HTML `<form>` 元素**表示文档中的一个区域，此区域包含交互控件，用于向 Web 服务器提交信息。

示例：

```html
<form action="" method="get" class="form-example" target="_blank">
  <div class="form-example">
    <!-- 注意这里的label标签，等会仔细讲解以下-->
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required>
  </div>
  <div class="form-example">
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required>
  </div>
  <div class="form-example">
    <input type="submit" value="Subscribe!">
  </div>
</form>
```



## fieldset标签(不常用)

`MDN`：**HTML `<fieldset>`** 元素用于对表单中的控制元素进行分组（也包括 label 元素）。

就是可以将表单项一项一项的框起来，看起来还蛮好看的，但是平时使用率不是很高

如果要使用：详情请见MDN



## label标签(常用)

`MDN`：**HTML `<fieldset>`** 元素用于对表单中的控制元素进行分组（也包括 label 元素）。

这个标签就非常常用了，以前我们写input输入框时，前面的文字都是直接加上去，或者写个span标签，现在别人有自己的标签了

**核心：结合input一起使用**

将一个 `<label>` 和一个 `<input>`元素相关联主要有这些优点：

- 标签文本不仅与其相应的文本输入元素在视觉上相关联，程序中也是如此。 这意味着，当用户聚焦到这个表单输入元素时，屏幕阅读器可以读出标签，让使用辅助技术的用户更容易理解应输入什么数据。
- 你可以点击关联的标签来聚焦或者激活这个输入元素，就像直接点击输入元素一样。这扩大了元素的可点击区域，让包括使用触屏设备在内的用户更容易激活这个元素。

将一个 `<label>` 和一个 `<input>` 元素匹配在一起，你需要给 `<input>` 一个 `id` 属性。而 `<label>` 需要一个 `for` 属性，其值和 `<input>` 的 `id` 一样。

注意：**不一定需要必须匹配，但是为了优化和方便残障人士，还是建议匹配上**

示例：

```html
<div class="preference">
    <label for="cheese">Do you like cheese?</label>
    <input type="checkbox" name="cheese" id="cheese">
</div>

<div class="preference">
    <label for="peas">Do you like peas?</label>
    <input type="checkbox" name="peas" id="peas">
</div>

```





## input标签(最常用)

`MDN`：**HTML `<input>` 元素**用于为基于 Web 的表单创建交互式控件，以便接受来自用户的数据; 可以使用各种类型的输入数据和控件小部件，具体取决于设备和[用户代理](https://developer.mozilla.org/zh-CN/docs/Glossary/User_agent)。

input标签可以说是表单类的核心了，既能变成输入框，又能变成checkbox，还能变成上传文件按钮，也能变成一个button，可以说是孙悟空（72变）了

它有一大堆的：type类型，和一大堆的属性，全都在MDN上可以看到，可以去查



## button标签

`MDN`：**HTML `<button>` 元素**表示一个可点击的按钮，可以用在[表单](https://developer.mozilla.org/en-US/docs/Learn/Forms)或文档其它需要使用简单标准按钮的地方。 默认情况下，HTML 按钮的显示样式接近于 [user agent](https://developer.mozilla.org/zh-CN/docs/Glossary/User_agent) 所在的宿主系统平台（用户操作系统）的按钮， 但你可以使用 [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) 来改变按钮的样貌。



注意：默认情况下，button标签自带表单的提交效果，点击就会让表单submit





## textarea标签

`MDN`：**HTML `<textarea>` 元素**表示一个多行纯文本编辑控件，当你希望用户输入一段相当长的、不限格式的文本，例如评论或反馈表单中的一段意见时，这很有用。

上述例子展示了 `<textarea>` 的几个特点：

示例：

```html
<label for="story">Tell us your story:</label>

<textarea id="story" name="story"
          rows="5" cols="33">
It was a dark and stormy night...
</textarea>
```



- 为了提高可访问性（accessibility），用于将 `<textarea>`与一个 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/label) 关联的 `id` 属性。
- `name` 属性，用于设置随表单一同提交到服务器的相关数据的名字。
- `rows` 和 `cols` 属性，用于声明 `<textarea>` 的精确尺寸。这对于一致性非常有帮助，因为不同浏览器的默认值常常不一样。
- 位于开始标签和闭合标签之间的默认内容。`<textarea>` 不支持 `value` 属性。

`<textarea>` 还可以使用 `<input>` 中的一些常见属性，如`autocomplete`, `autofocus`, `disabled`, `placeholder`, `readonly`, 和 `required`。