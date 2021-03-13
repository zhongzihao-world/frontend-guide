## HTML 规范

### 缩进

统一两个空格缩进

### 命名规范

- class 应以功能或内容命名，不以表现形式命名；
- class 与 id 单词字母小写，多个单词组成时，采用中划线-分隔；
- 使用唯一的 id 作为 Javascript hook, 同时避免创建无样式信息的 class；

#### DOCTYPE 声明

HTML 文件必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明：

```Html
<!DOCTYPE html>
```

### meta 标签

- 统一使用 “UTF-8” 编码

```Html
<meta charset="utf-8">
```

- SEO 优化

```Html
<!-- 页面关键词 -->
<meta name ="keywords" content =""/>
<!-- 页面描述 -->
<meta name ="description" content ="">
<!-- 网页作者 -->
<meta name ="author" content ="">
```

- 优先使用 IE 最新版本和 Chrome

```Html
<meta http-equiv ="X-UA-Compatible" content ="IE = edge，chrome = 1">
```

- 为移动设备添加视口

```Html
<!-- device-width 是指这个设备最理想的 viewport 宽度 -->
<!-- initial-scale=1.0 是指初始化的时候缩放大小是1，也就是不缩放 -->
<!-- user-scalable=0 是指禁止用户进行缩放 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

- 禁止自动识别页面中有可能是电话格式的数字

```Html
<meta name="format-detection" content="telephone=no">
```

**团队约定：**

pc 端：

```Html
<meta charset="utf-8">
<meta name="keywords" content="your keywords">
<meta name="description" content="your description">
<meta name="author" content="author,email address">
<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
```

移动端：

```Html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<meta name="format-detection" content="telephone=no">
```

### 标签

html 标签分为以下几类：

- 自闭合标签（self-closing），无需闭合。例如：area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr 等 )
- 闭合标签（closing tag），需闭合 。例如：textarea、title、h、div、span 等

**团队约定：**

- 所有具有开始标签和结束标签的元素都必须要写上起止标签，某些允许省略开始标签或和束标签的元素亦都要写上
- 自闭合标签不要加上结束标签
- 自定义标签的名字必须包含一个破折号（-），`<x-tags>`、`<my-element>`和`<my-awesome-app>`都是正确的名字，而`<tabs>`和`<foo_bar>`是不正确的。这样的限制使得 HTML 解析器可以分辨那些是标准元素，哪些是自定义元素
- 自定义标签必须写上开始标签和闭合标签
- 尽量减少标签数量

### 元素属性

- 元素属性值使用双引号语法

**推荐：**

```Html
<input type="text">
```

**不推荐：**

```Html
<input type=text>
<input type='text'>
```

### 代码嵌套

- 块元素可以包含内联元素或某些块元素，但内联元素却不能包含块元素，它只能包含其它的内联元素
- 标题和段落中不能包含块，如：h1、h2、h3、h4、h5、h6、p、dt
- 块与内联不能并列，块级元素与块级元素并列、内嵌元素与内嵌元素并列
- 有些标签是固定的嵌套规则，比如 ul 包含 li、ol 包含 li、dl 包含 dt 和 dd 等等。

### 灵活使用伪类

不要让非内容信息污染了你的 HTML，打乱了 HTML 结构。可以使用:before、:after 等伪类元素

**推荐：**

HTML 代码

```Html
<!-- That is clean markup! -->
<span class="text-box">
  See the square next to me?
</span>

```

CSS 代码：

```css
/* We use a :before pseudo element to solve the design problem of placing a colored square in front of the text content */
.text-box:before {
  content: '';
  display: inline-block;
  width: 1rem;
  height: 1rem;
  background-color: red;
}
```

**不推荐：**

HTML 代码：

```Html
<!-- We should not introduce an additional element just to solve a design problem  -->
<span class="text-box">
  <span class="square"></span>
  See the square next to me?
</span>
```

CSS 代码：

```css
.text-box > .square {
  display: inline-block;
  width: 1rem;
  height: 1rem;
  background-color: red;
}
```

### 特殊符号必须使用转义符

| 符号 | 描述 |  转义符  |
| :--: | :--: | :------: |
|      | 空格 | `&nbsp;` |
|  <   | 小于 |  `&lt;`  |
|  >   | 大于 |  `&gt;`  |
|  &   |  和  | `&amp;`  |
|  "   | 引号 | `&quot;` |

### 纯数字输入框

使用 type="tel" 而不是 type="number"

```Html
<input type="tel">
```

### 类型属性

不需要为 CSS、JS 指定类型属性，HTML5 中默认已包含。

**推荐：**

```Html
<link rel="stylesheet" href="" >
<script src=""></script>
```

**不推荐：**

```Html
<link rel="stylesheet" type="text/css" href="" >
<script type="text/javascript" src="" ></script>
```

### 注释规范

**单行注释**

- 一般用于简单的描述，如某些状态描述、属性描述等
- 注释内容前后各一个空格字符，注释位于要注释代码的上面，单独占一行

**推荐：**

```Html
<!-- Comment Text -->
<div>...</div>
```

**不推荐：**

```Html
<div>...</div><!-- Comment Text -->

<div><!-- Comment Text -->
    ...
</div>
```

**模块注释**

- 注释内容前后各一个空格字符
- `<!-- S Comment Text -->`表示模块开始
- `<!-- E Comment Text -->`表示模块结束，模块与模块之间相隔一行
- 模块注释内部嵌套模块注释，`<!-- /Comment Text -->`

推荐：

```Html
<!-- S Comment Text A -->
<div class="mod_a">

    <div class="mod_b">
        ...
    </div>
    <!-- /mod_b -->

    <div class="mod_c">
    	...
    </div>
    <!-- /mod_c -->

</div>
<!-- E Comment Text A -->

<!-- S Comment Text D -->
<div class="mod_d">
    ...
</div>
<!-- E Comment Text D -->
```

### 语义化

- 没有 CSS 的 HTML 是一个语义系统而不是 UI 系统
- 通常情况下，每个标签都是有语义的
- 语义化的 HTML 结构，有助于机器（搜索引擎）理解，另一方面多人协作时，能迅速了解开发者意图
- 建议页面中多使用语义化标签，而不是整个页面以 div 构成

#### 常见标签语义：

|    标签     |                                      语义                                      |
| :---------: | :----------------------------------------------------------------------------: |
|    `<p>`    |                                      段落                                      |
|   `<hn>`    |                                  标题(h1~h6)                                   |
|   `<ul>`    |                                    无序列表                                    |
|   `<ol>`    |                                    有序列表                                    |
|   `<nav>`   |                      标记导航，仅对文档中重要的链接群使用                      |
|  `<main>`   |     页面主要内容，一个页面只能使用一次。如果是 web 应用，则包围其主要功能      |
| `<article>` |                 定义外部的内容，其中的内容独立于文档的其余部分                 |
| `<section>` |   定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。    |
|  `<aside>`  | 定义其所处内容之外的内容。如侧栏、文章的一组链接、广告、友情链接、相关产品列表 |
| `<header>`  |                页眉通常包括网站标志、主导航、全站链接以及搜索框                |
| `<footer>`  |                 页脚，只有当父级是 body 时，才是整个页面的页脚                 |
| `<figure>`  |   规定独立的流内容（图像、图表、照片、代码等等）（默认有 40px 左右 margin）    |
