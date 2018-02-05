## 安装 ##

### ruby安装 ###
ruby的下载地址：
[http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/ "ruby下载地址")

![勾选第二项](https://i.imgur.com/AX5GiFr.png)

### Sass安装 ###
安装完ruby，请在菜单开始栏中找到ruby，打开Start Command Prompt with Ruby，
![打开命令](https://i.imgur.com/ZEjTa22.png)

然后在命令行中输入：
    
	`
	gem install sass
	`
按回车确认，等待一段时间会提示你sass安装成功。
如果要安装beta版本（测试版本），可以在命令行中输入：

   
	gem install sass --pre

查看sass版本的命令：

    `
	sass -v
	`




https://www.sassmeister.com/ 
在线编译scss网站.

scss 不能直接通过link标签引入文件,只不过是预处理工具,需要编译成sa
css的文件引入.

## 语法
以美元符号"$"开头
```
    $width:300px;
```
sass的变量包含三个部分:
1.声明变量的符号'$'
2.变量名称
3.赋予变量的值

## 局部变量与全局变量

```
    $color:red;//定义全局变量(在选择器 函数 混合宏的外面定义的变量为全局
    变量)
    .block{
        max-width:$color;//调用全局变量
    }
    em{
        $color:red;//定义局部变量
        a {
           color:$color//调用局部变量
        }
    }
    span{
        color:$color//调用全局变量
    }
```

## 什么时候声明变量
感觉确有必要的情况下,满足下面情况可以创建新变量:
1.该值至少重复出现了两次.
2.该值至少可能会被更新一次.
3.该值所有的表现与变量有关.
基本上,没有理由声明一个永远也不需要更新或者只在一个地方使用的变量.


## 嵌套

### 选择器嵌套
假设有这一段结构:
```
<header>
<nav>
    <a href=“##”>Home</a>
    <a href=“##”>About</a>
    <a href=“##”>Blog</a>
</nav>
<header>
```
想选中 header 中的 a 标签，在写 CSS 会这样写：
```
nav a {
  color:red;
}

header nav a {
  color:green;
}
```
那么在 Sass 中，就可以使用选择器的嵌套来实现：
```
nav {
  a {
    color: red;

    header & {
      color:green;
    }
  }  
}
```
就是为了方便组件化开发设计的。
```
nav {
    a {
        color: red;
        header & {
            color: green;
        }
        div.class1 & {
            color:yellow;
        }
        div.class2 & {
            color: aqua;
        }
    }
}
```

### 属性嵌套
css中有一些属性前缀相同,只是后缀不一样,比如border-top/
border-right 与这个类似的属性还有margin,padding等属性
```
.box {
    border-top: 1px solid red;
    border-bottom: 1px solid green;
}
```
sass中可以这样写:
```
.box {
  border: {
   top: 1px solid red;
   bottom: 1px solid green;
  }
}
```

## 混合宏

### 声明混合宏
如果整个网站有几处小样式类似,比如颜色,字体,在sass中可以使用
变量统一处理,那么这种选择还是不错的.
在sass中,使用'@mixin'来声明一个混合宏.如:
```
@mixin border-radius{
    -webkit-border-radius: 5px;
    border-radius: 5px;
}
```
其中@mixin 是用来声明混合宏的关键字,border-radius 是混合宏的名称,
大括号里面是复用样式的代码.

### 调用混合宏
实际调用中,匹配一个关键字'@include'调用声明好的混合宏.

比如上例有定义好的混合宏'border-radius',可以这样调用:
```
button {
    @include border-radius;
}
```

这样编译出来的css:
```
button {
  -webkit-border-radius: 3px;
  border-radius: 3px;
}
```

### 混合宏的参数--传一个不带值的参数

传一个不带值的参数,比如:
```
@mixin border-radius($radius){
  -webkit-border-radius: $radius;
  border-radius: $radius;
}
```

在调用的时候可以给这个混合宏一个参数值:
```
.box {
  @include border-radius(3px);
}
```
编译出来的css:
```
.box {
  -webkit-border-radius: 3px;
  border-radius: 3px;
}
```

### 混合宏的参数--传多个参数
Sass 混合宏除了能传一个参数之外,还可以传递多个参数:
```
@mixin center($width,$height){
  width: $width;
  height: $height;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -($height) / 2;
  margin-left: -($width) / 2;
}
```
实际调用:
```
.box-center {
  @include center(500px,300px);
}
```
编译出来的css:
```
.box-center {
  width: 500px;
  height: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -150px;
  margin-left: -250px;
}
```

### 扩展/继承
sass可以通过关键字 '@extend' 继承已存在的样式块,实现代码的继承.
```
//SCSS
.btn {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
  @extend .btn;
}

.btn-second {
  background-color: orange;
  color: #fff;
  @extend .btn;
}
```

编译结果:
```
//CSS
.btn, .btn-primary, .btn-second {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
}

.btn-second {
  background-clor: orange;
  color: #fff;
}
```
示例代码可以看出,sass继承可以继承样式块中所有的样式代码,
编译出来的css会将选择器合并在一起,形成组合选择器:

```
.btn, .btn-primary, .btn-second {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}
```

### 占位符%placeholder
可以取代以前的css中的基类造成的代码冗余,因为占位符代码如果不被
@extend调用的话,不会产生任何代码.
比如:

```
//SCSS
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}

.btn {
  @extend %mt5;
  @extend %pt5;
}

.block {
  @extend %mt5;

  span {
    @extend %pt5;
  }
}
```
编译出来的css:

```
//CSS
.btn, .block {
  margin-top: 5px;
}

.btn, .block span {
  padding-top: 5px;
}
```

编译出来的css代码可以看出,通过@extend调用占位符,编译出来的代码
会将相同的代码合并在一起.这是我们希望看到的效果,让代码更干净.

## Sass 注释
1.类似css的注释方式,使用'/*'开头,结尾使用"*/"
2.类似JS的柱释方式使用'//'

前者在编译出来的CSS显示,后者在编译出来的css不显示.


## Sass运算 加法和减法

```
.box {
  width: 20px + 20px;
}
```
请保持单位一致.














