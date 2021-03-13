## JavaScript 性能优化

### 加载和执行

1、 `</body>`闭合标签之前，将所有的`<script>` 标签放在页面底部，确保在脚步执行之前页面已经完成渲染。

2、 合并脚本。下载单个 100KB 的文件将比下载 4 个 25KB 的文件更快，因此页面标签的`<script>`标签越少，加载也就越快，响应也越迅速。无论外链文件或者内嵌脚本。

3、 使用无阻塞下载 `javascript` 的方法，即在页面加载完成后才加载 `Javascript` 代码，以下有几种无阻塞下载方法：

- 使用`<script>`的 `defer` 属性（只有 IE4+和 FireFox3.5+支持）

```html
<script src="file.js" defer></script>
```

- 使用动态创建的`<script>`元素来下载并执行代码

  ```javascript
  // 通过监听script元素接收完成事件来获得脚本加载完成时的状态
  // 封装标准和IE特有的实现方法函数
  function loadscript(url, callback) {
    let script = document.createElement('script');
    if (script.readyState) {
      //IE
      script.onreadystatechange = function() {
        if (
          script.readyState === 'loaded' ||
          script.readyState === 'complete'
        ) {
          script.onreadystatechange = null;
          callback();
        }
      };
    } else {
      // 其他浏览器
      scripy.onload = function() {
        callback();
      };
    }
    script.src = url;
    document.getElementsByTagName('head')[0].appendchild(script);
  }
  // 使用上述封装函数加载文件
  loadscript('file.js', function() {
    console.log('file is loaded ～');
  });
  ```

- 使用 XHR 对象下载 `javascript`代码并注入页面中

  ```javascript
  let xhr = new XMLHttpRequest();
  xhr.open('get', 'file.js', true);
  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4) {
      if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        let script = document.createElement('script');
        script.text = xhr.responseText;
        document.body.appendChild(script);
      }
    }
  };
  ```

**总结：**

向页面中添加大量 `JavaScript` 的推荐做法：先添加动态加载所需代码，然后加载初始化页面所需剩下代码

### 数据存储

1、访问字面量和局部变量的速度比访问数组元素和对象成员快，因此尽量使用字面量和局部变量，减少数组项和对象成员的使用。

2、尽可能使用局部变量。如果某个跨作用域的值在函数中被引用一次以上，把它存储到局部变量中。

```javascript
// 不推荐
function initFn() {
  // document.getElementById('myImg')多次被引用
  document.getElementById('myImg').src = 'photo.png';
  document.getElementById('myImg').alt = 'photo';
}

// 推荐
function initFn() {
  // 改写
  let myImg = document.getElementById('myImg');
  myImg.src = 'photo.png';
  myImg.alt = 'photo';
}
```

3、避免使用 with 语句，它会改变执行环境作用域链。try-catch 中的 catch 也有同样的影响，看实际情况中进行运用。

4、属性和方法在原型链中的位置越深，访问的速度越慢

**总结：**

把常用的对象成员、数组元素、跨域变量保存在局部变量中

### DOM

1、最小化 DOM 访问次数，如果需要多次访问某个 DOM 节点，使用局部变量存储它的引用。

```JavaScript
// 不推荐
function innerHTMLLoop(){
  for(let i = 0; i < 5; i++){
    document.getElementById('myDiv').innerHTML += 'a';
  }
}

// 推荐
function innerHTMLLoop(){
  let content = '';
  for(let i = 0; i < 5; i++){
    content += 'a';
  }
  document.getElementById('myDiv').innerHTML = content;
}
```

2、如果需要在迭代中使用 HTML 集合，把它的长度缓存到一个变量中；如果需要多次操作集合，把它拷贝到一个数组中。

- HTML 集合有`document.getElementsByName()`,`document.getElementsClassName()`,`document.getElementsByTagName()`,`document.images`,`document.links`,`document.forms`等等，返回的是类似数组的列表，但是它以一种“假定实时态”实时存在，即当底层文档对象更新时，它也会自动更新。

  ```javascript
  // 死循环
  let divs = document.getElementsByTagName('div');
  for (let i = 0; i < divs.length; i++) {
    document.body.appendChild(document.createElement('div'));
  }
  ```

- 当遍历一个集合时，把集合存储在局部变量中，并把 length 缓存在循环外部

  ```javascript
  // 推荐
  function collectNodes() {
    let coll = document.getElementsByTagName('div');
    let len = coll.length;
    let nodeName = '';
    let tagName = '';
    let el = null;
    for (let i = 0; i < len; i++) {
      el = coll[i];
      nodeName = el.nodeName;
      tagName = el.tagName;
    }
    return { nodeName, tagName };
  }
  ```

3、减少重绘和重排，批量修改样式时，“离线”操作 `DOM` 树，使用缓存，并减少访问布局信息的次数。

- 当页面布局和几何属性改变时需要“重排”。下列情况均会发生“重排”：

  1、添加或删除可见的 `DOM` 元素

  2、元素位置发生改变

  3、元素尺寸改变（包括：外边距、内边距、边框厚度、宽度、高度等属性改变）

  4、内容改变，如文本改变或图片被另一个不同尺寸的图片代替

  5、页面渲染器初始化

  6、浏览器窗口改变

- 完成“重排”后，浏览器会重新绘制受影响的部分到屏幕中，过程为“重绘”。

- 减少“重绘”和“重排”

  1、使元素脱离文档流

  2、对其应用多次改变

  3、把元素带回文档中

  ```javascript
  // 第一种方式：隐藏元素，应用修改，重新显示
  let ul = document.getElementById('nyList');
  ul.style.display = 'none';
  // 向ul中添加附加数据
  appendDataToElement(ul, data);
  ul.style.display = 'block';

  // 第二种方式：使用文档片段（document fragment）在当前DOM之外构建一个子树，再把它拷贝到文档(推荐)
  let fragment = document.createDocumentFragment();
  // 向fragment中添加附加数据
  appendDataToElement(ul, data);
  document.getElementById('myList').appendChild(fragment);

  // 第三种方式：将原始元素拷贝到一个脱离文档的节点中，修改副本，完成后再替换原始元素
  let old = document.getElementById('myList');
  let clone = old.cloneNode(true);
  // 向clone中添加附加数据
  appendDataToElement(ul, data);
  old.parentNode.replaceChild(clone, old);
  ```

4、使用事件委托减少事件处理器的数量

```javascript
// Event对象提供了一个属性叫target，可以返回事件的目标节点，我们成为事件源，也就是说，target就可以表示为当前的事件操作的dom，但是不是真正操作dom，当然，这个是有兼容性的，标准浏览器用ev.target，IE浏览器用event.srcElement。
myLi.onclick = function(e) {
  let e = e || window.event;
  let target = ev.target || ev.srcElement;
  if (target.nodeName.toLowerCase() == 'li') {
    alert(target.innerHTML);
  }
};
```

### 算法和流程控制

- 选择时间复杂度低的算法
- 避免使用 for-in 循环，除非需要遍历一个属性数量未知的对象
- 使用 if-else 要确保最可能出现的条件放在首位，条件数量大时，优先使用 switch
- 使用查找表。查找表相对于 if-else 和 switch，不近速度快，而且可读性好
  ```JavaScript
  // 将返回值集合存入数组
  let results = [result0,result1,result2,result3,result4,result5,result6,result7,result8,result9,result10,];
  // 返回当前结果
  return results[value];
  ```

### 构建和部署

- 合并 JavaScript 文件以减少 HTTP 请求数

- 使用 webpack 压缩 JavaScript 文件

- 使用 CDN 提供 JavaScript 文件；CDN 不仅可以提升性能，也会为你管理文件的压缩和缓存
