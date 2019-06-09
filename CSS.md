# CSS

## Font

### font-family

每个font-family 包含一组有共同特征的字体，共有5个字体系列：

- sans-serif 无衬线字体
	- Verdana
	- Arial Black
	- Arial
	- Trebuchet MS
	- Geneva
- serif 有衬线字体
	- 	Times
	-  Times New Roman
	-  Georgia
- monospace 包含了固定宽度的字符，像打印机打出来的
	- courier
	- courier new
	- Andale Mono
- cursive 看似手写的字体
	- Comic Sans
	- apple chancery
- fantasy
	- LAST NINJA
	- Impact

### @font-face

```
@font-face {
	font-family: "Emblema One";
	src: url("EmblemaOne-Regular.woff"),
	url("EmblemaOne-Regular.ttf");
}
```

### font-size

```
body {font-size: 14px;}
h1 {font-size: 150%;}
h2 {font-size: 1.2em;}
```

指定web字体大小的通常方法：

- 先为<body>指定一个大小 xx-small/x-small/small/medium/large/x-large/xx-large
- 我后相对于这个大小设置所有其他字体大小

```
body { font-size: small; }
h1 { font-size: 150%; }
h2 { font-size:120%; }
```

### font-weight
font-weight

- lighter
- normal
- bold
- bolder

### font-style

font-style

- italic / not italic;
- oblique / not oblique;

### text-decoration
- line-through
- underline
- overline
- none

## Color

```
body {
background-color: silver;
background-color: rgb(80%, 40%, 0%);
background-color: rgb(204, 102, 0);
background-color: #cc6600;
}
```

## box model
CSS将每个元素看作由一个盒子表示

- 最里面是内容区(content area)
- 内容区的外部是内边距(padding)
- 内边距周围可能放置一个可选的边框(border)
- 在边框的外部是外边距(margin)
- 四边的简称顺序是：上、右、下、左
- 两边的简称顺序是：上下、左右

## 多CSS表
在html中使用多CSS表时，排在下面的CSS表被应用

另一种情况是适配不同的终端：

```
<link href = "lounge-mobile.css" rel="stylesheet" media = "screen and (max-device-width: 480px)">
    
<link href = "lounge-print.css" rel = "stylesheet" media = "print">
```
还有一种解决思路是，直接在CSS中规定横样式

```
@media screen and (min-device-width: 481px) {
	#guarantee {}
}

@media screen and (max-device-width: 480px) {
	#guarantee {}
}
```