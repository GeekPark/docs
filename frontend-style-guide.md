# 极客公园前端风格指南

这是一个适用于[极客公园](http://www.geekpark.net)的风格指南。
使用方法：打印，每看完一条并想明白为什么后，打勾。

TODO：
  * [ ] 基本开发流程和资料索引
  * [ ] 工具上的规范
  * [ ] 响应式设计

## 0. 基本要求
* 缩进使用**长度为2的空格**，在任何情况下不使用`Tab`
* 没有尾空格
* 使用UTF-8编码（no BOM）[1](#为什么不使用BOM)
* html、css、js文件全部采用小写，并以`-`进行分隔，如：`good-name.js` [2](为什么采用`-`命名文件而不是`_`)

## 1. Why?
良好的风格，可以：
- 增强了可读性，间接增强了代码的可维护性。方便团队成员间、开源社区间进行交流
- 避免没有必要的错误

**规范不是限制人身自由，想清楚每一条规范是为什么而在。**


## 2. HTML
在开发中，我们使用[Slim](http://www.slim-lang.com)作为模板。
### a) 风格
* 使用双引号，当双引号中的内容含有引号时，使用单引号[3](#为什么使用双引号)
* 当一个标签具有多属性时，使用`#{exp}`
  * **BAD** `a target="_blank" href=topic_path(@topic)
  * **GOOD** `a target="_blank" href="#{topic_path(@topic)}"`
* 标签内的值要折行，但是保持可读
  * **BAD**

  ```
  td.operation
    | 状态：
    = @user.status_text
  ```

  * **NOT BAD** 当嵌套层级过多时，使用它

  ```
  td.operation = "状态：#{@user.status_text}"
  ```

  * **GOOD**

  ```
  td.operation
    | 状态：#{@user.status_text}
  ```
### b) 标签
* 根据语义使用标签，不滥用`<div>`和`<span>`，如：
  * 拒绝使用<br>调整样式
  * `<ul>`,`<ol>`,`<dl>`来表示列表
* 自闭合（self-closing）标签，不要在后面加`/`[参考](http://stackoverflow.com/questions/3558119/are-self-closing-tags-valid-in-html5)，如：
  * **GOOD** `<img src="img-path">`
  * **BAD** `<img src="img-path"/>`
* 避免无意义的标签
  * **BAD**

    ```
    <span class="avatar"><img src="img-path"></span>
    ```
  * **GOOD**

    ```
    <img class="avatar" src="img-path">
    ```
* `<table>`使用`<thead>`、`<tbody>`和`<tfoot>`（可省略），`<tfoot>`置于`<tbody>`前

  ```
  <table>
    <thead>
      <tr>
        <th scope="col">Table header 1</th>
        <th scope="col">Table header 2</th>
      </tr>
    </thead>
    <tfoot>
      <tr>
        <td>Table footer 1</td>
        <td>Table footer 2</td>
      </tr>
    </tfoot>
    <tbody>
      <tr>
        <td>Table data 1</td>
        <td>Table data 2</td>
      </tr>
    </tbody>
  </table>
  ```
### c) 属性
  * 如果要给元素赋予样式，使用`class`，如果是`js`绑定事件，使用`id`。当不能避免时，`class`加上`js-`前缀。这样的好处是：
      - 样式与逻辑分离，方便样式复用。相同样式的元素不代表具有相同逻辑
      - 易调试。一目了然这个元素是否有绑定
  * Boolean值不必在后面加上赋值符号，不填写则为`false`如：

    ```
    <label><input type=checkbox checked name=cheese disabled> Cheese</label>
    ```
  * 按钮需要有`type`属性，提交为`type="submit"`，普通按钮为`type="button"`
  * （风格建议）属性顺序：
    * id
    * class
    * name
    * data-*
    * src, for, type, href
    * title, alt
    * aria-*, role
### d) 图片
* 小图及logo等，使用GIF/PNG-8。如果图片大于200kb请考虑使用jpg交错式图片。
* 其他图片使用PNG-8
### 其他
* 所有的HTML文件，必须以<!DOCTYPE html>开头，slim代码：`doctype html`
* HTML文件标签中必须声明`<html lang="zh">`
* 指定字符编码`<meta charset="UTF-8">`
* 兼容IE请使用`<meta http-equiv="X-UA-Compatible" content="IE=Edge">`
* 引入CSS和JS无须在标签中指明type，如`text/css`
* 将CSS置于JS文件前

## 3. CSS
开发中，我们使用`scss`，注意是`sCss`，而不是`sAss`
### a) 文件组织（Rails）
* 文件分割不以页面为单位，而以功能为单位
* 外部文件及第三方类库，`vendor/assets/stylesheets`
* 常规，`app/assets/stylesheets`。
  当项目规格较大时，根据功能建立文件夹进行存放。避免直接置于`app/assets/stylesheets`下
* `public`文件夹中的内容，不会进行预编译

  ```
  源文件：public/test.png
  对应页面位置：www.geekpark.net/test.png
  ```
* 文件引用，要使用helper时使用`@import`，否则使用`require`

  ```css
  //= require_tree ./plugins
  //= require my_awesome_styles

  @import "with_mixin";
  ```
* media查询属性放置于单独文件
### b) 编码风格
#### 选择器
* 不进行引起歧义和不必要的缩写
  * **GOOD** navigation -> nav
  * **BAD** author -> athr //只减少了2个字符，却可能让人看不懂
* 标签名不与id、class等选择器一起使用，如：
  * **BAD** `span.ugly`

* 尽量避免使用`#`作为选择器，避免直接使用标签名作为选择器
* 使用`-`而不是`_`，不论是`id`还是`class`
  * **BAD** `.bad_name`
  * **GOOD** `.good-name`
* 在`{`前面加空格，`}`要新起一行，每个属性独占一行
  * **BAD**

    ```
    .hidden{display: none;}
    ```
  * **GOOD**

    ```
    .hidden {
      display: none;
    }
    ```
* 当选择多个元素时，每个元素独立占用一行
  * **NOT GOOD** `.block-a, .block-b`
  * **GOOD** 
  ```
  .block-a,
  .block-b {
    ...
  }
  ```
* `.js-*`用于控制由JS生成的样式
#### 空格及缩进
* **禁止使用空格**，使用2空格进行替换。
* 在`:`后面加空格
  * **BAD** `display:none;`
  * **GOOD** `display: none;`
* 规则之间加入空行

#### 其他
* SCSS使用中，避免无意义的嵌套，**最大层数不能超过3层**，如果超过请重构
* 赋空值时不要加上单位
  * **BAD** `margin: 0px;`
  * **GOOD** `margin: 0;`
* 使用`//`来注释代码，而非`/**/`

### c) 属性
* 减少`dispaly:inline-block`、`float`、`position`的使用
* 尽量采用简写模式，如：
  * **NOT GOOD** `background-image: url(//example.com)`
  * **GOOD** `background: url(//example.com)`
* `background`，忽略协议，`url`中不使用引号
  * **NOT GOOD** `background: url(http://example.com)`
  * **NOT GOOD 2** `background: url("//example.com")`
  * **RECOMMENDATION** `background: url(//example.com)`
* css声明顺序：
   * Positioning
   * Box model 盒模型
   * Typographic 排版（font等）
   * Visual 外观
* （建议）属性按照字典序排序

### Rails相关
* Rails环境下，非`css`结尾的文件且没有被sprockets引用的文件，如需调用，将其加入precompile列表
* 访问`assets`中的文件，请使用[assets helper]('https://github.com/rails/sass-rails#asset-helpers')
  * **WRONG** `background: url("assets/bg.png")`
  * **RIGHT** `background: asset-url("bg.png")`
    or
    `background: asset-data-url("bg.png")`


## 4. Javascript
开发时以[js-hint](http://www.jshint.com/)作为指导
### 变量和命名
* 变量命名使用驼峰命名（likeThis），构建函数首字母大写（MyObject）。有些时候例外：
  * `resultID` // 没有严格按照驼峰，因为`resultId`容易出现歧义

* 变量声明必须使用`var`，文件头部加入'use strict'，可以使用`var a = 1, b = 2;`
* `{`和不必换行

  ```
  block {
    ...
  }
  ```
* 变量赋值放在函数头部，所有的函数在使用前定义
* 创建一个类，将全部的全局变量存在其中，参考`geekpark.js`
### 风格
* 优先选择单引号，如果需要包含单引号，则使用双引号
* 函数调用时，函数名和`(`之间无需空格：`foo(bar)`；
  函数名与参数序列之间，没有空格`function foo(params)`
  所有其他语法元素与左括号之间，**都有一个空格**
* `(`，`[`，`/`，`+`，`-`前，不省略分号，常见错误
  * **Error**

  ```javascript
  x + y
  (function (){...})();
  // 等同于 x + y(function (){...})();
  ```

  ```
  x = {
    key: value
  }
  (function (){...})
  ```
* JS文件中如果一定要混入HTML请这么写：

  ```
  var template = ['<ul>', '<li>, </li>', '</ul>'].join('');
  ```
### 不能
* 不使用构造函数
  * **BAD** `var foo = new Array;`
  * **GOOD** `var foo = [];`
* 不使用`==`，使用`===`
* 不对变量直接进行`undefined`判断，而是使用`typeof`
  * **BAD** `console.log(person === undefined);`
  * **GOOD** `console.log(typeof(person) === 'undefined');`
* 不使用自增符号（`--`，`++`）
* 不能省略`{}`
* 避免不必要的代码合并，造成代码歧义
* 不是用IE的条件注释
* 不改变内置对象的prototype
* 禁止使用`eval`

  ```
  var a, b = 1;  // 等同于b=1; var a=1;
  ```

## 0. 响应式
TODO


## Why do you waste a lot of time wondering why?
    
###### 为什么不使用BOM
`BOM`早期用于区分UTF-8文件编码，现在既不必须，也不推荐使用。采用`BOM`会让你的代码在被别人修改时出现问题。
[whats-different-between-utf-8-and-utf-8-without-bom](http://stackoverflow.com/questions/2223882/whats-different-between-utf-8-and-utf-8-without-bom)

###### 为什么采用`-`命名文件而不是`_`
因为觉得漂亮。在SEO中有些含有`_`的链接不能被正确处理。[Why Not Recommended](http://blog.woorank.com/2013/04/underscores-in-urls-why-are-they-not-recommended/)

###### 为什么使用双引号
根据w3.org的标准来看，单引号和双引号并没有区别，但是我们在开发中，html和css统一使用双引号
[single-vs-double-quotes-vs](http://stackoverflow.com/questions/2373074/single-vs-double-quotes-vs)
