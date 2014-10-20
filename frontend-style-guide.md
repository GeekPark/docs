# 极客公园前端风格指南

这是一个适用于[极客公园](http://www.geekpark.net)的风格指南。

## 1. Why?
良好的风格，可以：
- 增强了可读性，间接增强了代码的可维护性。方便团队成员间、开源社区间进行交流
- 避免没有必要的错误

## 2. HTML
在开发中，我们使用[Slim](http://www.slim-lang.com)作为模板。
### a) 风格
* **禁止使用空格**，使用2空格进行替换。
* 使用双引号，当双引号中的内容含有引号时，使用单引号
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
### 其他
* 所有的HTML文件，必须以<!DOCTYPE html>开头，slim代码：`doctype html`
* 将CSS置于JS文件前

## 3. CSS
开发中，我们使用`scss`，注意是`sCss`，而不是`sAss`
### a) 文件组织
* 外部文件及第三方类库，`vendor/assets/stylesheets`
* 常规，`app/assets/stylesheets`。
  当项目规格较大时，根据功能建立文件夹进行存放。避免直接置于`app/assets/stylesheets`下
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

## 0. 响应式
