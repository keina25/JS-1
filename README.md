# JavaScript 总结

## 一. JavaScript 的历史

---

1. 含义：JavaScript（简称“JS”） 是一种具有函数优先的轻量级，解释型或即时编译型的高级编程语言。虽然它是作为开发 Web 页面的脚本语言而出名的，但是它也被用到了很多非浏览器环境中，JavaScript 基于原型编程、多范式的动态脚本语言，并且支持面向对象、命令式和声明式（如函数式编程）风格。
2. 来源：1995 年，网景招募了布兰登·艾克，目标是把 Scheme 语言嵌入到 Netscape Navigator 浏览器当中，布兰登·艾克在 1995 年 5 月仅花了十天时间就把原型设计出来了。
3. 标准：JavaScript 的标准是 ECMAScript 。截至 2012 年，所有浏览器都完整的支持 ECMAScript 5.1，旧版本的浏览器至少支持 ECMAScript 3 标准。2015 年 6 月 17 日，ECMA 国际组织发布了 ECMAScript 的第六版，该版本正式名称为 ECMAScript 2015，但通常被称为 ECMAScript 6 或者 ES6。

## 二. JavaScript 诞生记

---

布兰登·艾克：

- 1995 年，网景公司开发 JS 功能,同年，HTML 和 CSS 刚发明
- 1998 年，协助成立了 Mozilla.org
- 网景公司破产，成立 Mozilla 基金会，维护 Firefox
- 2014 年，布兰登晋升为 Mozilla 的 CEO，但是不到十天时间就被赶下台
- 2015 年，成立 Brave 公司，开发保护用户隐私

JavaScript 诞生：

- 布兰登临危受命，
- 公司要求给浏览器添加脚本功能
- 公司想要蹭 Java 流量，两者是同一时间出现的
- 布兰登花了 10 天时间设计 JS 最初版本

## 三. JavaScript 的 10 个设计缺陷

---

- 设计阶段过于仓促：因为布兰登花了 10 天时间设计的 JS 语言，没有更多时间去调试，所以 JS 并不是一个完整，体系的语言
- 没有先例：Javascript 同时结合了函数式编程和面向对象编程的特点，这很可能是历史上的第一例。而且直到今天为止，Javascript 仍然是世界上唯一使用 Prototype 继承模型的主要语言。这使得它没有设计先例可以参考。
- 过早的标准化：Javascript 的发展非常快，根本没有时间调整设计，Javascript 的规格还没来及调整，就固化了

### 10 个设计缺陷：

1. 不适合开发大型程序：

   Javascript 没有名称空间（namespace），很难模块化；没有如何将代码分布在多个文件的规范；允许同名函数的重复定义，后面的定义可以覆盖前面的定义，很不利于模块化加载

2. 非常小的标准库：

   Javascript 提供的标准函数库非常小，只能完成一些基本操作，很多功能都不具备

3. null 和 undefined：

   null 属于对象（object）的一种，意思是该对象为空；undefined 则是一种数据类型，表示未定义。

```
typeof null; // object

typeof undefined; // undefined
```

两者非常容易混淆，但是含义完全不同。

```
var foo;

　　alert(foo == null); // true

　　alert(foo == undefined); // true

　　alert(foo === null); // false

　　alert(foo === undefined); // true
```

在编程实践中，null 几乎没用，根本不应该设计它。

4. 全局变量难以控制:

   Javascript 的全局变量，在所有模块中都是可见的；任何一个函数内部都可以生成全局变量，这大大加剧了程序的复杂性。

```
　a = 1;

　　(function(){

　　　　b=2;

　　　　alert(a);

　　})(); // 1

　　alert(b); //2
```

5. 自动插入行尾分号

   Javascript 的所有语句，都必须以分号结尾。但是，如果你忘记加分号，解释器并不报错，而是为你自动加上分号。有时候，这会导致一些难以发现的错误。

   比如，下面这个函数根本无法达到预期的结果，返回值不是一个对象，而是 undefined。

```
function(){

　　　　return
　　　　　　{
　　　　　　　　i=1
　　　　　　};

　　}
```

原因是解释器自动在 return 语句后面加上了分号。

```
　　function(){

　　　　return;
　　　　　　{
　　　　　　　　i=1
　　　　　　};

　　}
```

6. 加号运算符

+号作为运算符，有两个含义，可以表示数字与数字的和，也可以表示字符与字符的连接。

```
　　alert(1+10); // 11

　　alert("1"+"10"); // 110
```

如果一个操作项是字符，另一个操作项是数字，则数字自动转化为字符。

```
　　alert(1+"10"); // 110

　　alert("10"+1); // 101
```

这样的设计，不必要地加剧了运算的复杂性，完全可以另行设置一个字符连接的运算符。

7. NaN
   NaN 是一种数字，表示超出了解释器的极限。它有一些很奇怪的特性：

```
　　NaN === NaN; //false

　　NaN !== NaN; //true

　　alert( 1 + NaN ); // NaN
```

与其设计 NaN，不如解释器直接报错，反而有利于简化程序。

8. 数组和对象的区分

由于 Javascript 的数组也属于对象（object），所以要区分一个对象到底是不是数组，相当麻烦。Douglas Crockford 的代码是这样的：

```
　　if ( arr &&
　　　　typeof arr === 'object' &&
　　　　typeof arr.length === 'number' &&
　　　　!arr.propertyIsEnumerable('length')){

　　　　alert("arr is an array");

　　}
```

9.  == 和 ===

==用来判断两个值是否相等。当两个值类型不同时，会发生自动转换，得到的结果非常不符合直觉。

```
　　"" == "0" // false

　　0 == "" // true

　　0 == "0" // true

　　false == "false" // false

　　false == "0" // true

　　false == undefined // false

　　false == null // false

　　null == undefined // true

　　" \t\r\n" == 0 // true
```

因此，推荐任何时候都使用"==="（精确判断）比较符。

10. 基本类型的包装对象

Javascript 有三种基本数据类型：字符串、数字和布尔值。它们都有相应的建构函数，可以生成字符串对象、数字对象和布尔值对象。

```
　　new Boolean(false);

　　new Number(1234);

　　new String("Hello World");
```

与基本数据类型对应的对象类型，作用很小，造成的混淆却很大。

```
　　alert( typeof 1234); // number

　　alert( typeof new Number(1234)); // object
```

关于 Javascript 的更多怪异行为，请参见[Javascript Garden](http://bonsaiden.github.com/JavaScript-Garden/zh/)和[wtfjs.com](https://wtfjs.com/)。

> 1. 阮一峰（http://www.ruanyifeng.com/blog/2011/06/10_design_defects_in_javascript.html）
> 2. 百度百科(https://baike.baidu.com/item/javascript#4)
> 3. 维基百科（https://zh.wikipedia.org/wiki/JavaScript#%E5%8E%86%E5%8F%B2）
