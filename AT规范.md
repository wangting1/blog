# 由@keyframes引起的@规则探究

最初是在使用animation的@keyframes实现动画时，少打了一个字母所以上网查了一下@keyframes发现css中还有不少相似的@规则，下面就我找到的@规则做一个简单介绍。

### @keyframes
- keyframes即关键帧。animation属性则可以利用@keyframes将指定时间段内的动画进行精细的划分。
语法结构为：```@keyframes animationname {keyframes-selector {css-styles;}}```
	- animationname：动画名称。
	- keyframes-selector：用来划分动画的时长，一般是百分数的形式，0%，10%，100%，还有from和to的形式，from相当于0%，to相当于100%。
	- css-styles：与keyframes-selector对应的css样式。
- 如：以下动画可以实现元素在制定时间内逐渐由可见变为不可见的状态。

```@keyframes testanimations{
		from{opacity:1;}
		to{opacity:0;}
	}
```

### @import
- @import用于导入外部的样式文件。如：```@import url("b.css");```@import放到样式代码的最前面，否则它将会不起作用
- 使用的语法结构如下：

```
@import url("global.css");
@import url(global.css);
@import "global.css";
//以上3种方式都有效。当使用url(url)的方式时，包住路径的引号可有可无；当直接使用url时，包住路径的引号必须保留。
```
指定媒体查询：

```
@import url(example.css) screen and (min-width:800px);
@import url(example.css) screen and (width:800px),(color);
@import url(example.css) screen and (min-device-width:500px) and (max-device-width:1024px);
//IE8不支持@import指定媒体查询。
```
- 在本地开发时有利于css的模块化，然后用工具进行合并压缩，不然上线的时候会增加请求。对于@import和Link的区别有兴趣的可以参考[http://www.kuqin.com/webpagedesign/20090429/48868.html](http://www.kuqin.com/webpagedesign/20090429/48868.html)

### @charset
- @charset <charset>;指定样式表使用的字符编码。如：@charset "utf-8";
- 由于html中一般都会有meta，```<meta charset="utf-8">```所以css中不加@charset对页面也不会有什么影响。

### @media
- 使用 @media 查询，你可以针对不同的媒体类型定义不同的样式，对于响应式布局非常好用，如：

 ```
  @media screen and (max-width: 1024px) {
    body {
        background-color:red;
    }
}
```

- 其语法如下： ```@media mediatype and|not|only (media feature) {
    CSS-Code;
}```

- 对于Retina屏幕判断：

```
@media only screen and (-webkit-min-device-pixel-ratio:1.5),
only screen and (-webkit-min-device-pixel-ratio:2),
only screen and (min-resolution:2dppx) {
	//Retina屏幕
    }
```

# @font-face
- 设置嵌入HTML文档的字体
- 语法：@font-face{font-family:name;src:<url>;sRules;}如：```@font-face{
	font-family:YH;
	src:url(http://domain/fonts/MSYH.TTF);
}```

# @page
- 设置页面容器的版式，方向，边空等。
- 语法：@page <label> <pseudo-classes>{ sRules }

# @support
- 其语法如下：
```
@supports <条件规则> {
  /* 特殊样式规则 */
}
```
- 基本属性的判断：

```
@supports (display: flex) {
  div { display: flex; }
}
```
- 加上逻辑判断：
	- ```@supports not (display: flex){#container div{float:left;}}```你的浏览器不支持“display:flex”属性，那么你可以使用“float:left”来替代。
	- ```@supports (column-width: 20rem) and (column-span: all) {
  div { column-width: 20rem }    
  div h2 { column-span: all }
  div h2 + p { margin-top: 0; }
}```如果你的浏览器同时支持“column-width:20rem”和“column-span:all”两个条件，浏览器将会调用下面的样式。
	- ```@supports (display: -webkit-flex) or (display: -moz-flex) or (display: flex) {
  section {
    display: -webkit-flex;
    display: -moz-flex;
    display: flex;
  }           
}```

- 用js判断浏览器的兼容性：```var supportsCSS = !!((window.CSS && window.CSS.supports) || window.supportsCSS || false);	```
