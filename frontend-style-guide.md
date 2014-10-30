# 极客公园前端风格指南

这是一个适用于[极客公园](http://www.geekpark.net)的风格指南。

TODO：
  * [ ] 基本开发流程和资料索引
  * [ ] 工具上的规范
  * [ ] 响应式设计

## 1. Why?
良好的风格，可以：
- 增强了可读性，间接增强了代码的可维护性。方便团队成员间、开源社区间进行交流
- 避免没有必要的错误


## 2. HTML
在开发中，我们使用[Slim](http://www.slim-lang.com)作为模板。
### a) 风格
* **禁止使用空格**，使用2空格进行替换。
* 使用双引号，当双引号中的内容含有引号时，使用单引号
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
* 自闭合（self-closing）标签，不要在后面加`/`，如：
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
  * Boolean值不必在后面加上赋值符号，不填写则为`false`如：

    ```
    <label><input type=checkbox checked name=cheese disabled> Cheese</label>
    ```
  * 按钮需要有`type`属性，提交为`type="submit"`，普通按钮为`type="button"`
### d) 图片
* 小图及logo等，使用GIF/PNG-8。如果图片大于200kb请考虑使用jpg渐进式图片。
* 其他图片使用PNG-8
### 其他
* 所有的HTML文件，必须以<!DOCTYPE html>开头，slim代码：`doctype html`
* 将CSS置于JS文件前

## 3. CSS
开发中，我们使用`scss`，注意是`sCss`，而不是`sAss`
### a) 文件组织（Rails）
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
### b) 编码风格
#### 选择器
* 尽量避免使用`#`作为选择器
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
#### 空格及缩进
* **禁止使用空格**，使用2空格进行替换。
* 在`:`后面加空格
  * **BAD** `display:none;`
  * **GOOD** `display: none;`

#### 其他
* SCSS使用中，避免无意义的嵌套，**最大层数不能超过3层**，如果超过请重构
* 赋空值时要加上单位
  * **BAD** `margin: 0;`
  * **GOOD** `margin: 0px;`
* 使用`//`来注释代码，而非`/**/`

### Rails
* Rails环境下，非`css`结尾的文件且没有被sprockets引用的文件，如需调用，将其加入precompile列表
* 访问`assets`中的文件，请使用[assets helper]('https://github.com/rails/sass-rails#asset-helpers')
  * **WRONG** `background: url("assets/bg.png")`
  * **RIGHT** `background: asset-url("bg.png")`
    or
    `background: asset-data-url("bg.png")`


## 4. Javascript
开发时以[js-hint](http://www.jshint.com/)作为指导
### 变量和命名
* 变量命名使用驼峰命名（likeThis），构建函数首字母大写（MyObject）
* 变量声明必须使用`var`，文件头部加入'use strict'
* `{`和不必换行

  ```
  block {
    ...
  }
  ```
* 变量赋值放在函数头部，所有的函数在使用前定义
* 创建一个类，将全部的全局变量存在其中，参考`geekpark.js`
### 风格
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
### 不能
* 不使用`==`，使用`===`
* 不使用自增符号（`--`，`++`）
* 不能省略`{}`
* 避免不必要的代码合并，造成代码歧义
* 禁止使用`eval`

  ```
  var a, b = 1;  // 等同于b=1; var a=1;
  ```


## 0. 响应式
TODO
## Code Guide by  GeekPark team
       
       规范说明
       总规范
         html规范
         css规范
         javascript规范
### 规范说明
   
   此为前端开发团队遵循和约定的代码书写规范，意在提高代码的规范性和可维护性。
   
### 总规范
   1. 忽略（Omit）协议：如 `background: url(http://www.google.com/images/example);` 应该写`background: url(//www.google.com/images/example);`以方便http与https协议切换，除非必须使用某个协议
   2. 缩进使用两个空格，不要使用tab
   3. 标签属性使用小写
   4. 尾部不要留有空格，以防diff
   5. 使用 UTF-8 (no BOM)，文档中也加入 <meta charset=”utf-8″>
   6. 项目名、js、css、html文件全部采用小写方式， 以中划线分隔。 比如： my-project-name
   
### html规范
  html需要注意的就是语义化，注重结构，div滥用是绝对不允许存在的，在layout的过程中，对所构造的网页的结构，以及响应式的布局。遵循能使用html5标签的首选html5标签，这样使网页的表现更表现。
 
##### 语法
  1. 在属性上，使用双引号，不要使用单引号。
  2. 不要在自动闭合标签结尾处使用斜线 -(HTML5 规范)指出他们是可选的。
  3. 不要忽略可选的关闭标签（例如，`</li>` 和`</body>`）
  4. 使用html5的规范`<!DOCTYPE html>`
  5. Language attribute - Sitepoint只是给出了语言代码的大类，比如说中文就只给出了ZH，但是没有区分香港，台湾，大陆等。
     
        <html lang="en-us"> <!-- ... --> </html>
  6. IE compatibility mode  不同doctype在不同浏览器下的不同渲染模式
  
         <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  7. 字符编码  通过声明一个明确的字符编码，让浏览器轻松、快速的确定适合网页内容的渲染方式。
         
          <head> <meta charset="UTF-8"> </head>
  8. 多媒体标签向后兼容，记得加上alt属性
  9. 由于utf-8编码的使用，某些记号无需转码，如 `& mdash;, & rdquo;, or & #x263a;`当然`<`或是`&`除外
 
##### html5规范
  1. 根据 HTML5 规范, 通常在引入 CSS 和 JavaScript 时不需要指明 type，因为 text/css 和 text/javascript 分别是他们的默认值。    
  2. 尽量遵循 HTML 标准和语义，但是不应该以浪费实用性作为代价。任何时候都要用尽量小的复杂度和尽量少的标签来解决问题。
  3. 属性顺序－HTML 属性应该按照特定的顺序出现以保证易读性。
    * id
    * class
    * name
    * data-*
    * src, for, type, href
    * title, alt
    * aria-*, role
    
             Classes 是为高可复用组件设计的，理论上他们应处在第一位。Ids 更加具体而且应该尽量少使用（例如, 页内书签），所以他们处在第二位。但为了突出id的重要性， 把id放到了第一位。

  4. 不要为 Boolean 属性添加取值
  5. 减少标签数量－在编写 HTML 代码时，需要尽量避免多余的父节点。很多时候，需要通过迭代和重构来使 HTML 变得更少。
  6. 注意HTML5规定的可省略标签
  7. 在 JavaScript 文件中生成标签让内容变得更难查找，更难编辑，性能更差。应该尽量避免这种情况的出现。
  
     html的书写采用slim模板来书写，其他规范详见slim规范

### Css 规范
  1. 减少：dispaly:inline-block、float、position的使用
  2. 命名采用双英文拼写，中间用－进行连接,正确使用缩写，
     例如navigation就可以缩写为nav，而author就不要缩写
  3. 减少css代码的迭代，例如margin:0 auto，实际上我们需要的是margin-left:auto;和marign-right:auto;虽然后者比前者多一行代码，但是它不会迭代这个div下面的样式，在下面的margin－top可能就不会再去填坑，所以，认清需要什么，css就写什么，不要把无意义的代码加上，
  4. 对于google浏览器的输入框，还未输入时，存在光标不居中的问题，遇到input框，采用div包含无框的input来实现
  5. id与类名前不必加上标签类型
  6. 属性尽量使用简写形式，如font或background等
  7. url()中不要加入引号，例如@import url(//www.google.com/css/go.css);
  8. 属性采用字典序申明，包括前缀如moz安排在webkit之前
  9. 属性：与值之间用一个空格分开，例如font-weight: bold;
 10. 为每个选择符及每个属性申明单独使用一行
        
        h1,
        h2,
        h3{
          font-weight: normal;
          line-height: 1.2;
        }
 11. 规则之间用一个空行分开 
 12. 逗号分隔的取值，都应该在逗号之后增加一个空格。比如说box-shadow
 13. 不要在颜色值 rgb() rgba() hsl() hsla()和 rect()中增加空格，并且不要带有取值前面不必要的 0 (比如，使用 .5 替代 0.5)。
 14. 所有的十六进制值都应该使用小写字母，例如 #fff。因为小写字母有更多样的外形，在浏览文档时，他们能够更轻松的被区分开来。
 15. 尽可能使用短的十六进制数值，例如使用 #fff 替代#ffffff。
 16. 不要为 0 指明单位，比如使用 margin: 0; 而不是margin: 0px;
 17. css声明顺序
    * Positioning
    * Box model 盒模型
    * Typographic 排版
    * Visual 外观
    
    Positioning 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。盒模型紧跟其后，因为他决定了一个组件的大小和位置。其他属性只在组件 内部 起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面。
 18. 不要使用 @import导入css文件
 19. 媒体查询位置－尽量将媒体查询的位置靠近他们相关的规则。这样方便更改，对比样式。不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部。这样做只会让大家以后更容易忘记他们。
 20. 单条声明的声明块－在一个声明块中只包含一条声明的情况下，为了易读性和快速编辑可以考虑移除其中的换行。所有包含多条声明的声明块应该分为多行。
 21. 属性简写－坚持限制属性取值简写的使用，属性简写需要你必须显式设置所有取值。常见的属性简写滥用包括:
    * padding
    * margin
    * font
    * background
    * border
    * border-radius
    
    大多数情况下，我们并不需要设置属性简写中包含的所有值。例如，HTML 头部只设置上下的 margin，所以如果需要，只设置这两个值。过度使用属性简写往往会导致更混乱的代码，其中包含不必要的重写和意想不到的副作用。
 22. 代码注释 - 代码是由人来编写和维护的。保证你的代码是描述性的，包含好的注释，并且容易被他人理解。好的代码注释传达上下文和目标。不要简单地重申组件或者 class 名称。
    
         /* Bad example */ 
           /* Modal header */ 
           .modal-header { ... } 
        /* Good example */
          /* Wrapping element for .modal-title and .modal-close */
           .modal-header { ... }
 23. Class 命名
     
     * 保持 Class 命名为全小写，可以使用短划线（不要使用下划线和 camelCase 命名）。短划线应该作为相关类的自然间断。(例如，.btn 和 .btn-danger)。
     * 避免过度使用简写。.btn 可以很好地描述 button，但是 .s 不能代表任何元素。
     * Class 的命名应该尽量短，也要尽量明确。
     * 使用有意义的名称；使用结构化或者作用目标相关，而不是抽象的名称。
     * 命名时使用最近的父节点或者父 class 作为前缀。
     * 使用 .js-* classes 来表示行为(相对于样式)，但是不要在 CSS 中包含这些 classes。
     
            /* Bad example */ 
            .t { ... } 
             .red { ... } 
             .header { ... } 
            /* Good example */ 
            .tweet { ... }
             .important { ... } 
             .tweet-header { ... }

 24. 选择器
    
    * 使用 classes 而不是通用元素标签来优化渲染性能。
    * 避免在经常出现的组件中使用一些属性选择器 (例如，[class^="..."])。浏览器性能会受到这些情况的影响。
    * 减少选择器的长度，每个组合选择器选择器的条目应该尽量控制在 3 个以内。
    * 只在必要的情况下使用后代选择器 (例如，没有使用带前缀 classes 的情况).
 25. 代码组织
    * 以组件为单位组织代码。
    * 制定一个一致的注释层级结构。
    * 使用一致的空白来分割代码块，这样做在查看大的文档时更有优势
    * 当使用多个 CSS 文件时，通过组件而不是页面来区分他们。页面会被重新排列，而组件移动就可以了。
    
### Javascript
##### Indentation,分号,单行长度
  1. 使用两个空格
  2. Statement 之后一律以分号结束，不可以省略，必须使用var，多个采用简写方式，写到一起
     
         var a = "abc",
             b = "123";
  3. 单行长度，理论上不要超过80列
  4. 如果需要换行，存在操作符的情况，一定在操作符后换行，然后换的行缩进2个空格，多次换行的话就没必要缩进了
  5. 空行
      * 方法之后加
      * 声明赋值：后加
      * 逻辑块之间加空行增加可读性
  6. 变量命名
      * 标准变量采用驼峰标识
      * 使用的ID的地方一定全大写,例：resultID
      * 使用的URL的地方一定全大写, 比如说 reportURL
      * 涉及Android的，一律大写第一个字母
      * 涉及iOS的，一律小写第一个，大写后两个字母
      * 构造函数，大写第一个字母
  7. 语句后记得用分号结尾;
  8. for in 不要用在遍历array上，只能用在object上
  9. 不要使用ie的条件注释
 10. 不要改变内置对象的prototype
 11. array与object创建使用字面量而不是包装构造函数
 12. 文件命名统一使用小写”.js”，同时推荐”-“而不是”_”
 13. 自定义的toString方法必须保证 1）正确性；2）没有副作用；否则很容易在assert时出错
 14. 可以推迟初始化
 15. 字符串相对于双引号，推荐使用单引号
 16. 永远不要直接使用undefined进行变量判断,使用字符串 "undefined" 对变量进行判断
        
        // Bad 
          var person;
          console.log(person === undefined);
          //true 
        // Good
          console.log(typeof person); 
          // "undefined"
 17. Object Literals
 
        // Bad 
          var team = new Team(); 
          team.title = "AlloyTeam"; 
          team.count = 25; 
        // Good
          var team = { title: "AlloyTeam", count: 25 };
     Araay也是如此	
    