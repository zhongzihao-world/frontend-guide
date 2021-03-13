# JavaScript 规范

> JavaScript 是一门弱类型语言，正因如此导致写法五花八门，这里整理了一些在编写代码时需要注意的事项

_`*` 星号表示建议遵守_

## 通用规范

### 文件编码

为了避免内容乱码，统一使用 `UTF-8` 编码保存。

_在文件结尾处，保留一个空行。_

### 代码检测

开启 `eslint` 代码规范和错误检查。

_eslint 可实现下面大部分的规范内容_

### 在严格模式模式下编码

```
'use strict';
```

## 类型规范

- js 数据类型有 string、number、boolean、null、undefined、array、function 和 object 这几种，不同数据类型有不同的存储方式，也对应有不用的使用方法，对于数据赋值要注意以下几点

  - 初始值类型要明确
  - 不要随意变换类型

- 类型检测`优先使用 typeof`。对象类型检测使用 `instanceof`。null 或 undefined 的检测使用 == null。

- 字符串开头和结束使用单引号 `'...string...'`

## 命名规范

- 变量命名采用小驼峰命名，如:addUser password studentID
- 常量命名采用单词所有字母大写,并用下划线分隔，如：FORM_NAME
- 对于对象、函数、和实例采用小驼峰（camelCase）命名法

  ```javascript
  // 对象
  let isObject = {};
  // 函数
  function isFun(){
   ...
  };
  // 实例
  let myBbj = new Object();
  ```

- 对于类命名或者构造函数，采用大驼峰命名 User() DateBase()

  ```javascript
  // 类
  class Point {
    ...
  };

  // 构造函数
  function User(options) {
    this.name = options.name;
  }

  let myBbj = new User({
    name: 'yup'
  });
  ```

## 代码规范

### 缩进

统一使用`两个空格缩进`，不推荐使用 tap 缩进。

### 引号

统一使用`单引号`。

### 换行

每个独立语句结束后必须换行。

### 分号

不得省略语句结束的分号

### 代码块

使用花括号包裹所有的多行代码块。

_单行 if 语句也必须使用花括号括住_

```js
// 推荐
if (true) {
  // TODO ...
}
```

```js
// 不推荐
if (true) // TODO ...
```

### 使用全等符号

在等号表达式中使用类型严格的 `===`和`!==`。使用 === 可以避免等于判断中隐式的类型转换。

```js
// 推荐
if (age === 30) {
  // ......
}
```

```js
// 不推荐
if (age == 30) {
  // ......
}
```

## 注释规范

### 单行注释

使用 `//`  作为单行注释。在评论对象上面另起一行使用单行注释。在注释内容前插入一个空格。

```js
// 单行注释
```

### 多行注释

以`/*`开头，`*/`结尾，注释内容前后加一个空格

```js
/*
 * 第一行注释
 * 第二行注释
 */
```

```js
/* 另外一种写法 */
```

### 方法注释

函数(方法)注释也是多行注释的一种，但是包含了特殊的注释要求，关键方法必须加注释。

```js
/**
 * 方法功能描述
 * @param {*} 参数
 * @param {*} 参数
 * @param {*} 参数
 * @param {*} 参数
 * @return 返回值
 */
```

### TODO 注释

使用  // TODO:  标注问题的解决方式。

```js
function Calculator() {
  // TODO: total should be configurable by an options param
}
```

## 性能优化

参考[JavaScript 性能优化](./source/JavaScript性能优化.md)
