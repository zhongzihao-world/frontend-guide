## nodeJs 规范

### 缩进

统一两个空格缩进

### 变量声明

局部变量一定要声明,避免全局污染

- 推荐使用`let`全面代替`var`，因为它创建了块级作用域变量（变量只在代码块内生效），尤其是`for`循环
- 建议自由在逻辑上是常量的情况才使用 `const`，它代表常量，定的同时必须赋值

### 单引号

使用 string 时，用单引号替代双引号（写 JSON 时除外）

**推荐**：

```javascript
var foo = 'bar';

var http = require('http');
```

不推荐:

```javascript
var foo = 'bar';

var http = require('http');
```

### 命名规范

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

### 赋值提升

在变量作用范围的最顶端声明变量

### 全等

比较操作时必须使用全等符号，'==='和'!=='

### 类型转换

- 在声明语句的最前端执行强制类型转换

  ```javascript
  // -> string:
  var totalScore = '' + this.reviewScore;
  ```

- 使用 `parseInt` 来进行整数的类型转换，并且始终提供一个基数.

  ```javascript
  var inputValue = '4';
  var val = parseInt(inputValue, 10);
  ```

### 使用字面表达式

用 `{}` ,`[]` 代替 `new Array` ，`new Object`

### Requires

- 建议使用以下顺序来组织 node 代码中的 require 代码

  1.core modules

  2.npm modules

  3.others

  ```javascript
  var http = require('http');
  var fs = require('fs');
  var async = require('async');
  var mongoose = require('mongoose');
  var Car = require('./models/Car');
  ```

* 引入模块时，不要加 .js 后缀

  ```javascript
  var Batmobil = require('./models/Car');
  ```

### 避免魔鬼数字的出现

魔鬼数字主要指在代码中没有具体含义的数字、字符串。影响了代码可读性，读者看到的数字无法理解其含义，从而难以理解程序的意图。当程序中出现的魔鬼数字过多时，代码的可维护性将会急剧下降，代码变得难以修改，并容易引入错误。

**要求将魔鬼数字定义为常量，并增加注释**

```javascript
// 接口成功返回的标识
const SUCCESS_CODE = 1;
// 接口失败返回的标识
const FAIL_CODE = 0;
$.ajax({
  url: baseUrl + 'v1/activity/getAct',
  cache: false,
  type: 'GET',
  dataType: 'json',
  success: function(result) {
    if (result.ret === SUCCESS_CODE) {
      ...
    }else if(result.ret === FAIL_CODE){
      ...
    }
  }
});
```

### 函数设计

- 从意义上应该是一个独立的功能模块

- 函数名要在一定程度上反映函数的功能

- 函数名要在一定程度上反映函数的功能

- 当函数参数不应该在函数体内部被修改时，应加上 `const` 声明

- 避免函数有过多的参数，参数个数尽量控制在 4 个以内

- 一个函数只做一件事，就要尽量保证抽象层级的一致性，所有语句尽量在一个粒度上。若在一个函数中处理多件事，不利于代码的重用

### 回调函数

在回调函数中要始终检测错误,并返回错误

```javascript
function getAnimals(done) {
  Animal.get(function(err, animals) {
    if (err) {
      // 返回错误
      return done(err);
    }
    // 建议回调函数中使用描述性的参数
    return done(null, {
      dogs: animals.dogs,
      cats: animals.cats
    });
  });
}
```

### Try catch

在同步调用中多使用 Try catch 去捕获错误，如：JSON.parse()应该使用 try-catch 语句块

```javascript
var data;
try {
  data = JSON.parse(jsonAsAString);
} catch (e) {
  //handle error - hopefully not with a console.log ;)
  console.log(e);
}
```

## 构造函数

- 在原型链上增加属性，而不是覆写原型链

  **推荐：**

  ```javascript
  function Jedi() {
    console.log('new jedi');
  }

  Jedi.prototype.fight = function fight() {
    console.log('fighting');
  };

  Jedi.prototype.block = function block() {
    console.log('blocking');
  };
  ```

  **不推荐：**

  ```javascript
  function Jedi() {
    console.log('new jedi');
  }

  Jedi.prototype = {
    fight: function fight() {
      console.log('fighting');
    },

    block: function block() {
      console.log('blocking');
    }
  };
  ```

- 使用 `class` 关键字，并使用 `extends` 关键字来继承

  ```javascript
  class Queue {
    constructor(contents = []) {
      this._queue = [...contents];
    }
    pop() {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }
  }

  class PeekableQueue extends Queue {
    peek() {
      return this._queue[0];
    }
  }
  ```

- 在方法中返回 `this` 以方便链式调用

  **推荐：**

  ```JavaScript
  class Jedi {
    jump() {
      this.jumping = true;
      return this;
    }
    setHeight(height) {
      this.height = height;
      return this;
    }
  }
  const luke = new Jedi();
  luke.jump()
    .setHeight(20);
  ```

  **不推荐：**

  ```javascript
  Jedi.prototype.jump = function() {
    this.jumping = true;
    return true;
  };
  Jedi.prototype.setHeight = function(height) {
    this.height = height;
  };
  const luke = new Jedi();
  luke.jump(); // => true
  luke.setHeight(20); // => undefined
  ```

## 箭头函数

- 当你必须使用函数表达式（或传递一个匿名函数时），使用箭头函数符号（能够自动绑定`this`到父对象）

  **推荐：**

  ```JavaScript
  [1, 2, 3].map((x) => {
    return x * x
  });
  ```

  **不推荐：**

  ```javascript
  [1, 2, 3].map(function(x) {
    return x * x;
  });
  ```

- 总是用括号包裹参数，即便只有一个参数

  **推荐：**

  ```JavaScript
  let foo = (x) => x + 1;
  ```

  **不推荐：**

  ```javascript
  let foo = x => x + 1;
  ```

- 对于对象、类中的方法，使用增强的对象字面量

  **推荐：**

  ```JavaScript
   let foo = {
    bar () {
      // code
    }
  }
  ```

  **不推荐：**

  ```javascript
  let foo = {
    bar: () => {
      // code
    }
  };

  let foo = {
    bar: function() {
      // code
    }
  };
  ```

## `...`扩展运算符

- 函数参数永远不要使用类数组对象 `arguments`，使用 `...` 操作符来代替

  **推荐：**

  ```JavaScript
   function concatenateAll(...args) {
    return args.join('');
  }
  ```

  **不推荐：**

  ```javascript
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join('');
  }
  ```

- 使用 `...` 来拷贝数组

  **推荐：**

  ```JavaScript
   const itemsCopy = [...items];
  ```

  **不推荐：**

  ```javascript
  const len = items.length;
  const itemsCopy = [];
  let i;
  for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
  }
  ```

## 注释

- 使用`/** ... */`进行多行注释. 请在你们加入注释说明，指明参数和返回值的类型
  ```javascript
  /**
   * make() returns a new element
   * based on the passed in tag name
   * @param <String> tag
   * @return <Element> element
   */
  ```
- 使用 `//` 进行单行注释. 请用一个新行来添加注释。并在注释行前增加一个空行

  ```javascript
  // is current tab
  var active = true;

  function getType() {
    console.log('fetching type...');
    // set the default type to 'no type'
    var type = this._type || 'no type';
    return type;
  }
  ```
