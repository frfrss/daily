##  <\label>
<\label> 标签为 input 元素定义标注（标记）。

label 元素不会向用户呈现任何特殊效果。不过，它为鼠标用户改进了可用性。如果您在 label 元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。

### <\label> 标签的 for 属性应当与相关元素的 id 属性相同。
## <\input>的type属性
type 属性规定 input 元素的类型。

值|描述
:-:|:-:
button|定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）。
checkbox	|定义复选框。
file	|定义输入字段和 "浏览"按钮，供文件上传。
hidden|	定义隐藏的输入字段。
image	|定义图像形式的提交按钮。
password	|定义密码字段。该字段中的字符被掩码。
radio	|定义单选按钮。
reset	|定义重置按钮。重置按钮会清除表单中的所有数据。
submit	|定义提交按钮。提交按钮会把表单数据发送到服务器。
text	|定义单行的输入字段，用户可在其中输入文本。默认宽度为 20 个字符。
## <\link>
链接一个外部样式表：
```
<head>
<link rel="stylesheet" type="text/css" href="theme.css" />
</head>
```
