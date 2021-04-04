这本书定位在DOM入门，并未过多涉及JS的语法部分，适合对HTML和CSS有一定了解的人阅览

此书知识密度较低，仅用一篇文章讲述阅览心得

[TOC]



## JavaScript语法

JavaScript不必在每条语句后加`;`，直接换行同样可以完成语句的解释执行，但加上`;`可以增加程序的可读性

如果一行要塞上两条语句，这两条语句必须通过`;`隔开

```javascript
first statement; second statement;
```

JavaScript的注释形式和`C/C++`类似，通过符号`//`或`/*...*/`完成

不同的是JavaScript还可以通过`<!--`完成注释，其效果与`//`相同

JavaScript的变量不需要提前声明，不过为了程序的可读性，应当将变量声明在程序的开头

JavaScript的变量有作用域，在函数外声明的变量具有全局作用域，在函数内声明的变量作用域只有该函数内部

```javascript
var a="I'm a!";  //全局变量
function func1(){
    var b="I'm b!";  //局部变量，作用域为整个函数
    console.log(a);
}
func1();         //"I'm a!"
console.log(b);  //Error: b is not defined
```

变量还有块级作用域，只有在严格模式下才起作用，本书未提及

JavaScript是弱类型语言，以下代码完全合法

```javascript
var s="Hello,World!";
s=10;
```

JavaScript中的字符串可以使用成对的`"`或者`'`，两者完全等价，内部特殊符号使用转义字符`\`进行转义

数组声明语法如下

```javascript
var a=array();              //声明一个数组
var b=array(4);			    //声明一个长度为4的数组
var c=["join",1940,false];  //声明一个长度为3的数组
                            //其值依次为字符串"join"、整数1940和布尔值false
var d=array(123,234,436);
```

此外，数组支持嵌套

```javascript
var e=[1,3,[2,3]];
```

数组的第一个下标是0，支持使用方括号`[]`访问

与`C/C++`、`java`等语言不同，JavaScript的对象属性是运行时确定的，这使得我们可以使用对象方便地保存数据

```javascript
var date=Object();
date.year=2021;
date.month=1;
date.day=5;
```

JavaScript还支持使用花括号快速创建对象

```javascript
var date={year:2021,month:1,day:5};
```

对象同样支持嵌套

```javascript
var note=Object();
note.createDate=Date;
```

JavaScript支持诸如`+-*/`等操作符，其中`+`不仅可以用于数与数、字符串与字符串之间，还可以将字符串与数拼接到一起

```javascript
var s="This year is "+2021;
console.log(s);	//This year is 2021
```

JavaScript的`==`运算符十分宽松，即便是两个不同类型的操作数，如`0`和`false`，也能判断为true

因此，在绝大多数情况下，我们使用`===`和`!==`代替`==`和`!=`

```javascript
var resultA=(0==false);		//true
var resultB=(0===false);	//false
```

JavaScript同样有`while(){}`、`do{}while()`、`for(;;){}`、`if(){}else{}`等语句

JavaScript的函数通过`function`关键字声明，不需要标返回类型

```javascript
function add(a,b){
    return a+b;
}
```

最后，JavaScript中的对象并不只是保存数据那么简单，对象包裹的数据有两种`属性`和`方法`

* 属性是隶属于某个特定对象的变量
* 方法是只有某个特定对象才能调用的函数

属性和方法都能通过`.`操作符访问

```javascript
Object.property
Object.method()
```

通过`new`操作符可以创建对象的实例

```javascript
var a=new Object();
var b=new array();	//array也是对象
```

在JavaScript中，对象通过原型链连接在一起，这一点和基于继承的`C/C++`、`Java`有很大的不同

JavaScript和浏览器提供了许多有用的对象，因此在这本书中我们只需要使用对象，无需自行创建对象

## DOM

### DOM树

DOM将一份文档表示为一个节点树

DOM中有许多不同的节点，对我们来说最有用的是**元素节点**、**文本节点**和**属性节点**

```html
<p title="example" class="main common" id="exNode">
    This is a example
</p>
```

##### 元素节点

在html文档中，我们使用了诸如`<html>`、`<body>`、`<p>`之类元素，这些元素便是DOM的元素节点

元素节点层层包含，只有`<html>`不会被别的元素包含，它便是DOM树的根节点

##### 文本节点

仅有元素的html文档只是一个空白页面，不含有任何内容

在上面的例子中，`<p></p>`包含的This is a example被称为文本节点

文本节点包含于元素节点中，但不是所有元素节点含有文本节点

##### 属性节点

属性节点用于更具体地描述元素

上面的例子中，`<p>`的`title`、`class`、`id`属性被称作属性节点

### DOM方法

浏览器会提供给我们一些内建的对象，windows对象便是其中之一

windows对象代表窗口本身，这个对象的属性和方法被称为BOM

而DOM不同，它通过document对象进行编程

以下是document提供的几个方法，分别用于操作元素和属性

* getElementById
  这个方法需要一个id参数，并返回一个对象，对应着document中的一个独一无二的元素

  ```javascript
  var a=document.getElementById("exNode");
  ```

* getElementsByTagName
  这个方法同样接收一个参数，但返回一个对象数组，对应着document中所有名字相同的元素

  ```javascript
  var a=document.getElementsByTagName("p");	//对象数组而不是对象
  var p1=a[0];				//第一个p元素节点
  var p2=a[1];				//第二个p元素节点
  var l=a.length;				//document中p元素的数量
  ```

* getElementsByClassName
  这个方法接收一个参数，返回一个对象数组

  ```javascript
  var a=document.getElementsByClassName("main")[0];
  //可以接受多个class name，中间用空格隔开
  var b=document.getElementsByClassName("main common")[0];	
  ```

* getAttribute
  该方法用于获得元素节点的属性值
  它接收一个参数，为要查询的属性名，返回对应的属性值
  它与上述方法不同，并不是document的方法，而是元素节点的方法
  可以与上述三个方法配合使用

  ```javascript
  var a=document.getElementById("exNode");
  var s=a.getAttribute("title");	//example
  ```

* setAttribute
  该方法用于设置属性
  它接受两个参数，分别为属性名和要设置的属性值

  ```javascript
  var a=document.getElementById("exNode");
  console.log(a.getAttribute("title"));	//example
  a.setAttribute("title","I have been changed");
  console.log(a.getAttribute("title"));	//I have been changed
  ```

  