---
layout: post
title: Sass基础入门
categories: 前端
description: 世界上最成熟、最稳定、最强大的专业级CSS扩展语言。
keywords: Sass, css
---

Sass世界上最成熟、最稳定、最强大的专业级CSS扩展语言！是前端很通用的一种扩展语言，虽然我们是移动端开发，但也是有必要了解一下。

## 为什么用Sass而不是Less?

Bootstrap使用Less，但在即将发布的4.0却选择了Sass

------

## 是"SASS"还是"sass"还是"Sass"?

根据官网出现的频率，是"Sass"

------

## Sass值得学吗?

都来到这了，这话问的。如果你需要学习一个CSS预处理器，那就选择Sass吧，没错的

------



那就开始吧！



## 1.使用变量

*使用`$`符号来标识变量*

#### 1.1 变量声明

```css
$highlight-color: #FF99000;
$basic-border: 1px solid black;
```

**变量命名**：推荐使用中划线分割

*注：中划线或下划线是兼容的*

**作用域**：

1. 当变量定义在`css`规则块之外，那么在该样式表中均可使用

2. 当变量定义在`css`规则块内，那么该变量只能在此规则块内使用

   

#### 1.2 变量引用

凡是`css`属性的标准值（比如说1px或者bold）可存在的地方，变量就可以使用

```css
$highlight-color: #F90;
.selected {
  border: 1px solid $highlight-color;
}
 /* 编译后 */
.selected {
  border: 1px solid #F90;
}
```

**在声明变量时，变量值也可以引用其他变量**

```css
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
```

#### 1.3 默认值

一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值

使用`!default`标签表明：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值

```css
$link-color: blue;
$link-color: red;
a {
color: $link-color;
}
 /* 编译后 */
a {
color: red;
}
```

```css
$link-color: blue;
$link-color: red!default;
a {
color: $link-color;
}
 /* 编译后 */
a {
color: blue;
}
```



### 2.嵌套CSS 规则

#### 2.1 俄罗斯套娃

实际上是转为后代选择器，伪类不支持

```css
#content {
  background-color: #f5f5f5;
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}

 /* 编译后 */
#content { background-color: #f5f5f5; }
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

#### 2.2 父选择器的标识符&

```css
article a {
  color: blue;
  &:hover { color: red }
}
 /* 编译后 */
article a { color: blue }
article a:hover { color: red }
```

#### 2.3 群组选择器的嵌套

```css
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
 /* 编译后 */
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```

```css
nav, aside {
  a {color: blue}
}
 /* 编译后 */
nav a, aside a {color: blue}
```

#### 2.4 子组合选择器和同层组合选择器

同层相邻组合选择器`+`

同层全体组合选择器`~`

子组合选择器`>`

```css
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
 /* 编译后 */
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

#### 2.5 嵌套属性

嵌套属性的规则：把属性名从中划线-的地方断开，在根属性后边添加一个冒号`:`，紧跟一个`{ }`块，把子属性部分写在这个`{ }`块中

```css
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
 /* 编译后 */
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

使用`:`的好处就是可以指明例外规则

```css
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
 /* 编译后 */
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```

### 3.导入SASS文件

`sass`的`@import`规则在生成`css`文件时就把相关文件导入进来

![img](https://www.sass.hk/images/p1.png)

#### 3.1 局部文件

`sass`局部文件的文件名以下划线开头，局部文件不会在编译时单独编译输出`css`

它只用来被导入！

导入时，可以省略文件名开头的下划线。

```css
@import "themes/night-sky";/*导入themes/_night-sky.scss文件*/
```

#### 3.2 嵌套导入

`sass`允许`@import`命令写在`css`规则内，这样导入的局部文件中定义的所有变量和混合器就会在规则范围内生效

有一个名为`_blue-theme.scss`的局部文件，内容如下：

```css
aside {
  background: blue;
  color: white;
}
```

然后把它导入到一个CSS规则内，如下所示：

```css
.blue-theme {@import "blue-theme"}

//生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。

.blue-theme {
  aside {
    background: blue;
    color: #fff;
  }
}
```

#### 3.3 原生的CSS导入

下列三种情况下会生成原生CSS的`@import`，尽管这会造成浏览器解析`css`时的额外下载：

- 被导入文件的名字以`.css`结尾；
- 被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
- 被导入文件的名字是`CSS`的url()值。

### 4.注释

```hxml
body {
  color: #333; //这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

### 5.混合器

a.***混合器使用`@mixin`标识符定义***

```css
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

b.***通过`@include`来使用这个混合器***

```css
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

 /* 编译后 */
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

c.***混合器中不仅可以包含属性，也可以包含`css`规则，包含选择器和选择器中的属性***

```css
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```

d.***给混合器传参***

跟`JavaScript`的`function`很像

```css
/* 可以通过语法$name: default-value的声明形式设置参数默认值,默认值可以是任何有效的css属性值，甚至是其他参数的引用 */
@mixin link-colors($normal, $hover:red, $visited:$normal) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

当混合器被`@include`时，你可以把它当作一个`css`函数来传参

```css
a {
  @include link-colors(blue, red, green);
}
/*可以通过语法$name: value的形式指定每个参数的值*/
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}

/*Sass最终生成的是*/
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

### 6. 继承

继承:一，以子选择器修饰的html元素，最终效果会包括其父选择器，二，任何跟父选择器有关的组合选择器也会被以组合选择器的方式继承

```css
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```



## 参考

- [Sass中文网](https://www.sass.hk/guide/)
- [Github](https://github.com/sass/sass)
- [Sass-lang](http://sass-lang.com/)
