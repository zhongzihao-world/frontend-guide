# CSS 规范

> CSS 指层叠样式表 (Cascading Style Sheets)，定义如何显示 HTML 元素，但由于 CSS 天生全局性，随着项目复杂度增加，极易出现样式覆盖以及其它的问题。

_`*` 星号表示建议遵守_

## 1. 通用规范

### 文件编码

- 为了避免内容乱码，统一使用 `UTF-8` 编码保存。

- `*` 样式文件第一行设置字符集为 UTF-8

```
@charset 'UTF-8'; /* 注意字符集说明应在第一行 */
```

### 缩进规范

统一使用`两个空格缩进`

## 2. 初始化规范

- 不使用 UI 框架，由零开始搭建  
  从零开始搭建的情况下，使用[css 初始化文件](./source/ijunhai-reset.css) 进行样式初始化，在项目最开始的时候就引入，不要在开发中途引入，避免不可预知的样式冲突。

- 不使用 UI 框架，但使用了部分插件  
  插件往往都带有自己特有的样式，如富文本插件，在开发中途使用初始样式有可能导致样式错乱，所以不建议大范围的初始化，只需`简单进行初始化`即可。

```
* {
  padding: 0;
  margin: 0;
}
```

- 已使用 UI 框架  
  在明确知道需要使用 UI 框架的时候，`不使用第三方初始化样式`，不管是在项目开始前还是进行中，因为 UI 框架一般都自带初始化，额外引入了反而会影响原有效果。

## 3. 代码规范

### 命名规范

> class 应以功能或内容命名，不以表现形式命名  
> class 与 id 单词字母小写，多个单词组成时，采用中划线-分隔  
> 使用唯一的 id 作为 Javascript hook, 同时避免创建无样式信息的 class

### 代码风格

- 统一使用`展开格式`，不推荐紧凑格式

  ```css
  /* 展开格式 */
  .test {
    color: red;
    font-size: 12px;
  }
  ```

  ```css
  /* 紧凑格式 */
  .test {
    color: red;
    font-size: 12px;
  }
  ```

- 统一`两个空格`缩进

- 属性声明结尾加分号

- 选择器与左括号之间一个空格，属性冒号后一个空格

  ```css
  /* 推荐 */
  .test {
    color: red;
    font-size: 12px;
  }
  ```

  ```css
  /* 不推荐 */
  .test {
    color: red;
    font-size: 12px;
  }
  ```

- 不要为 0 指明单位

- 颜色值和属性值十六进制数值能用简写的尽量用简写

  ```
  /* 推荐 */
  .test {
    color: #fff;
  }
  ```

  ```
  /* 不推荐 */
  .test {
    color: #ffffff;
  }
  ```

- 引号使用

  url() 、属性选择符、属性值使用`单引号`。

- 清除浮动

  当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 clear 或触发 BFC 的方式进行 clearfix。尽量不使用增加空标签的方式。

  触发 BFC 的方式很多，常见的有：

  - float 非 none
  - position 非 static
  - overflow 非 visible

### 字体规范

- 对外商用网站，不要用`font-face`引入`微软雅黑`字体，避免侵权（包括图片内容）
- 需要在 Windows 平台显示的中文内容，其字号应不小于 `12px`

```
网站上使用 微软雅黑 字体有三种形式：

1、【侵权】图片中使用 微软雅黑 字体，比如网站头图
2、【安全】网站 CSS 用 font-family 声明网站使用 微软雅黑 字体，比如文章标题和正文
3、【侵权】网站通过 font-face 引用 微软雅黑 ，这种方式不常见
```

### 选择器规范

在严格遵照[BEM(Block Element Modifier)](http://getbem.com/)时，建议只使用类选择器，但 BEM 书写麻烦，所以建议如下

- 禁用通用选择器 \*
- 不使用无具体语义定义的标签选择器

### 属性顺序（建议）

CSS 属性顺序是 CSS 良好编码风格的一部分，有助于提高代码可读性，便于发现代码问题，有利于团队合作，但在项目中发现部分同学在书写属性顺序时较为随意，想到一个属性就写一个。

建议使用下列顺序进行书写

1. 定位属性（position、display、float、left、right）
2. 尺寸属性（width、height、padding、margin、border）
3. 字体属性（color、font、text-align）
4. 其他属性（background、cursor、outline）

目的是在浏览代码时，能逐步清晰目标元素的效果。

```css
.test {
  display: block;
  position: relative;
  float: left;
  width: 100px;
  height: 100px;
  margin: 0 10px;
  padding: 20px 0;
  font-size: 12px;
  color: #333;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 10px;
}
```

## 4. 注释规范

### 单行注释

注释以 `/*` 开始，以 `*/` 结束,注释内不能嵌套注释，注释内容`前后空一个空格`。

```css
/* 推荐的单行注释 */
```

```css
/*不推荐的单行注释*/
```

_注：在 sass 和 less 等预处理语言上也可以使用双斜线注释，但编译后注释内容不会出现在 css 文件中，所以建议统一使用`/* */`注释。_

### 模块注释

有时候我们需要对一个模块(一段代码块)进行功能性说明，并希望能明显区分其它代码，我们可以模块注释的方式。

注释以 `/*` 开始，以 `*/` 结束，`前后空一个空格`，第一行填写`描述`，最后一行行填写`分割线`。

```css
/* 推荐的模块注释
---------------------------------------------------- */
```

```css
/* 不推荐的模块注释 ---------------------------------------------------- */
```

### `*` 文件信息注释

如果需要对一个文件进行功能性说明，方便其他人快速明白该文件的作用，推荐在文件开头(字符集说明下)写入下列注释，注释内容包括文件描述、创建人、创建时间等。

```css
@charset "UTF-8";
/**
 * @desc 文件功能描述，方便其他人快速理解
 * @author 创建人
 * @date 创建时间
 */
```

## 5. 覆盖规范

- 尽可能`少用` importent
- vue 单文件组件统一使用 css/less/sass `scoped`
- 每个页面/组件需要有一个`全局唯一`的标识 id/class，属于它下面的样式都需要加上该唯一标识
- 避免全局修改已有样式，必须具体到页面上(通过权重)
- 禁用全匹配`*`选择器（特殊情况除外，如初始化）

_vue 单文件组件修改样式不生效可使用 `/deep/` 或 `>>>`_

## 6. 媒体查询

对于内部管理系统，商务多使用 ThinkPad 笔记本，屏幕分辨率为 `1366*768`。建议使用`Resolution Test`浏览器拓展进行浏览器窗口大小调试。

下载地址：http://www.cnplugins.com/devtool/resolution-test/download.html

常用尺寸如下

|  大小   |        描述         |
| :-----: | :-----------------: |
| ≥1366px | 大屏幕 大桌面显示器 |
| ≥1200px | 中等屏幕 桌面显示器 |
| ≥992px  | 中等屏幕 桌面显示器 |
| ≥768px  |     小屏幕 平板     |
| <768px  |    超小屏幕 手机    |

### 优先 PC 端

_默认按最大尺寸进行布局，当尺寸缩小时逐步变成移动端布局_

```
body {
  background: gray;
}
@media screen and (max-width: 1366px) {
  body {
    background: red;
  }
}
@media screen and (max-width: 1200px) {
  body {
    background: yellow;
  }
}
@media screen and (max-width: 920px) {
  body {
    background: green;
  }
}
@media screen and (max-width: 768px) {
  body {
    background: black;
  }
}
```

### 优先移动端

_默认按最小尺寸进行布局，当尺寸放大时逐步变成 PC 端布局_

```
body {
  background: gray;
}
@media (min-width: 768px) {
  body {
    background: red;
  }
}
@media (min-width: 920px) {
  body {
    background: green;
  }
}
@media (min-width: 1200px) {
  body {
    background: yellow;
  }
}
@media (min-width: 1366px) {
  body {
    background: red;
  }
}
```

_如果需要做打印样式进行适配，需要使用@media print_

```
@media print {
  body {
    background: #fff;
  }
}
```

## 7. 单位规范

CSS 单位有两种，分别是`绝对单位`和`相对单位`。

- 常用绝对单位

  - px：像素 (计算机屏幕上的一个点)
  - cm：厘米
  - in：英寸
  - pt：磅 (1 pt 等于 1/72 英寸)

- 常用相对单位
  - %：父元素百分比
  - vw：视口宽度百分比
  - vh：视口高度百分比
  - em：当前字体倍数
  - rem：根元素字体倍数
  - `*` rpx：微信小程序专用，规定屏幕宽为 750rpx

使用较多的单位有 px、%、rem 三种，_`建议 PC 端用 px 单位、移动端用 rem，需要具体控制尺寸还是使用 px`_

_ps：如果需要计算不同单位下的值，可以使用 css3 方法 calc()_

## 8. 兼容性规范

### 私有属性的使用

正是由于浏览器厂商的不同，导致了一些样式需要加前缀才生效，下面的常见的浏览器内核和前缀

|     浏览器     |  内核   |   前缀   |
| :------------: | :-----: | :------: |
|    Firefox     |  Gecko  |  -moz-   |
|     Chrome     | WebKit  | -webkit- |
|       IE       | Trident |   -ms-   |
|     Safari     | WebKit  | -webkit- |
|     Opera      | Presto  |   -o-    |
| 国内知名浏览器 | WebKit  | -webkit- |
| 常见手机浏览器 | WebKit  | -webkit- |

CSS3 浏览器私有前缀在前，标准前缀在后

```
.test {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -o-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```

_ps：在 webpack 环境下，可以使用 postcss-loader 自动添加私有前缀_

### `*` IE10 可用

**外部系统**

兼容到 `IE10`

**内部系统**

对于内部系统，一般都会要求使用最新版本 Chrome 进行访问，为了避免用户使用其他浏览器进行访问却无法正常使用，我们建议兼容到 `IE11` (功能完善，能正常进行业务操作)。

建议使用 360 浏览器进行版本切换调试。
