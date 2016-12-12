---
title: javascript中的变量和函数声明提升
date: 2016-12-08 17:06:37
tags: javascript
---

在Javascript中，函数及变量的声明都将被提升到函数的最顶部。声明函数是将函数的声明以及定义都提升，函数表达式和变量表达式只是将函数或者变量的声明提升到函数顶部，而函数表达式和变量的初始化将不被提升，如果不弄清楚这个问题，很可能在工作中带来麻烦。 
<!--more-->

## 变量声明提升

### 变量作用域

变量作用域指变量起作用的范围。变量分为全局变量和局部变量。全局变量在全局都拥有定义；而局部变量只能在函数体内有效,函数参数也是局部变量。 
在函数体内，同名的局部变量或者参数的优先级会高于全局变量。也就是说，如果函数内的局部变量或者参数和全局变量同名，那么全局变量将会被局部变量覆盖。 
**所有不使用var定义的变量都视为全局变量**

### 函数作用域和声明提前

JavaScript使用函数作用域:变量在声明它们的函数体内以及这个函数嵌套的任意函数体内都是有定义的;
JavaScript的函数作用是指在函数内声明的所有变量在函数体内始终是有定义的，**也就是说变量在声明之前已经可用**，所有这特性称为声明提前（hoisting），即JavaScript函数里的所有声明（**只是声明，但不涉及赋值**）都被提前到函数体的顶部，而变量赋值操作留在原来的位置。
*注释：声明提前是在JavaScript引擎的预编译时进行，是在代码开始运行之前。*
如下面例子：
```javascript
var scope = 'global';
function f(){
    console.log(scope);  //输出undefined 而不是 global
    var scope = 'local'; 
    console.log(scope);  //输出local
}
```
由于函数内声明提升，所以上面的代码实际上是这样的
```javascript
var scope = 'global';
function f(){
    var scope;    		//变量声明提升到函数顶部
    console.log(scope);
    scope = 'local';    //变量初始化依然保留在原来的位置
    console.log(scope);
}
```
经过这样变形之后，答案就就非常明显了。由于scope在第一个console.log(scope)语句之前就已经定义了，但是并没有赋值，因此此时scope的指是undefined.第二个console.log(scope)语句之前，scope已经完成赋值为’local’，所以输出的结果是local。

## 函数声明提升

### 函数的两种创建方式

* 函数声明语法

``` javascript
f('zhangc');
function f(name){
    console.log(name);
}
```
运行上面的程序，控制台能打印出zhangc。 

* 函数表达式语法*(产生副作用)*

``` javascript
f('zhangc');
var f = function(name){
    console.log(name);
}
```
运行上面的代码，会报错`Uncaught ReferenceError: f is not defined(…)`,错误信息显示说f没有被定义。 
为什么同样的代码，函数声明和函数表达式存在着差异呢？ 
这是因为，函数声明有一个非常重要的特征：函数声明提升，函数声明语句将会被提升到外部脚本或者外部函数作用域的顶部（是不是跟变量提升非常类似）。函数声明语句中的函数名是一个变量名,变量指向函数对象,和通过var声明变量一样,函数声明语句中的函数被显示地"提前"到了脚本或者函数的顶部,因此它们在整个脚本和函数内部是可见的.正是因为这个特征，所以可以把函数声明放在调用它的语句后面。如下面例子，最终的输出结果应该是什么？：

``` javascript
var getName = function(){
    console.log(2);
}
function getName (){
    console.log(1);
}
getName();
```
可能会有人觉得最后输出的结果是1。让我们来分析一下，这个例子涉及到了变量声明提升和函数声明提升。正如前面说到的函数声明提升，函数声明`function getName(){}`的声明会被提前到顶部。而函数表达式`var getName = function(){}`则表现出变量声明提升。因此在这种情况下，getName也是一个变量，因此这个变量的声明也将提升到顶部部，而变量的赋值依然保留在原来的位置。因此上面的函数可以转换成下面的样子:

```javascript
var getName;           //变量声明提升
function getName(){    //函数声明提升到顶部
    console.log(1);
}
getName = function(){   //变量赋值依然保留在原来的位置
    console.log(2);
}
getName();              // 最终输出：2
```
所以最终的输出结果是：2。在原来的例子中，函数声明虽然是在函数表达式后面，但由于函数声明提升到顶部，因此后面getName又被函数表达式的赋值操作给覆盖了，所以输出2。

## 结论

* 使用函数表达式语句即通过var定义函数),只有变量声明提前了--变量的初始化代码仍然在原来的位置;使用函数声明语句,函数名称和函数体均被提前;

* 变量声明提升：变量申明在进入执行上下文就完成了。只要变量在代码中进行了声明，无论它在哪个位置上进行声明， js引擎都会将它的声明放在范围作用域的顶部；

* 函数声明提升：执行代码之前会先读取函数声明，意味着可以把函数申明放在调用它的语句后面。只要函数在代码中进行了声明，无论它在哪个位置上进行声明， js引擎都会将它的声明放在范围作用域的顶部；可以在声明javaScript函数之前调用它;

* 变量or函数声明：函数声明会覆盖变量声明，但不会覆盖变量赋值。同一个名称标识a，即有变量声明var a，又有函数声明function a() {}，不管二者声明的顺序，函数声明会覆盖变量声明，也就是说，此时a的值是声明的函数function a() {}。注意：如果在变量声明的同时初始化a，或是之后对a进行赋值，

## 一个完整的例子

``` html
<html>
<head>
<title>函数提升</title>
<script language="javascript" type="text/javascript">
    
    //在全局对象中声明两个全局函数,反模式
    function foo(){
        alert("global foo");
    }
    
    function bar(){
        alert("global bar");
    }

    //定义全局变量
    var v = "global var";
    
    function hoistMe(){
        alert(typeof foo); //function
        alert(typeof bar); //undefined
        alert(v); //undefined

        //为什么bar函数和变量v是未定义而不是全局变量中定义的相应的函数变量呢？
         //因为函数里面定义了同名的函数和变量，无论在函数的任何位置定义这些函数和
         //和变量，它们都将被提升到函数的最顶部。
        
        foo(); //local foo
        bar(); //报错，缺少对象
        
        //函数声明，变量foo以及其实现被提升到hoistMe函数顶部
        function foo()
        {
            alert("local foo");
        }
        
        //函数表达式,仅变量bar被提升到函数顶部，实现没有被提升
        var bar = function()
        {
            alert("local bar");
        };

        
        //定义局部变量
         var v = "local";
    }
    
    (function(){
        hoistMe();  
    })();
    
   //函数表达式和变量表达式只是其声明被提升，函数声明是函数的声明和实现都被提升。
    /**由于函数提升的效果，hoistMe方法相当于
    function hoistMe(){
        //函数声明，变量foo以及其实现被提升到hoistMe函数顶部
        function foo(){
            alert("local foo");
        }
        
         //函数表达式,仅变量bar被提升到函数顶部，实现没有被提升(同变量提升)
        var bar = undefined;
        
        //变量声明被提升
         var v = undefined;
        
        alert(typeof foo); //function
        alert(typeof bar); //undefined
        
        foo(); //local foo
        bar(); //报错，缺少对象
       
        bar = function(){
            alert("local bar");
        };
      
       v = "local"; 
    }
    */
</script>

</head>
<body>
</body>
</html>
```
*内容整理自互联网*
