---
# Common-Defined params
title: "one20"
date: "2019-09-29"
description: "Example article description"
tags:
  - "Test"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
thumbnail: "img/placeholder.png" # Thumbnail image
comments: false # Enable Disqus comments for specific page
authorbox: false # Enable authorbox for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
---
<!-- TOC -->

- [JavaScript语言新发展](#javascript语言新发展)
  - [一、javascript函数式编程](#一javascript函数式编程)
  - [二、javaScript与QA测试工程师](#二javascript与qa测试工程师)
  - [三、JavaSCript与QA测试工程师补课](#三javascript与qa测试工程师补课)
  - [四、第一周实战](#四第一周实战)
  - [五、第一周试卷讲解](#五第一周试卷讲解)

<!-- /TOC -->

# JavaScript语言新发展

## 一、javascript函数式编程

1.函数式编程思维

- 范畴论  Category Theory
- - \1. 函数式编程是范畴论的数学分支，是一门复杂的数学，人为世界上所有的概念都可以抽象出一个范畴	
  - 2.彼此之间存在某种关系概念、事物、对象等，都构成范畴。任何事物只要找出他们之间的关系，就能定义
  - 3.箭头表示范畴成员之间的关系，正式的名称叫做“态射”（morphism）。范畴论认为，同一个范畴的所有成员，就是不同状态的“变形”（transform）。通过态射，一个成员可以变形成另一个成员
  - 注：所有成员是一个集合，变形关系是函数
- 函数式编程基础理论
- - 1.函数式编程(Function Programming)其实相对于计算机的历史而言，是一个非常古老的概念，甚至早于第一台计算机的诞生。函数式编程的基础模型来源于λ（lambda x=>x*2）演算，而λ演算并非设计于在计算机上执行，它是在20世纪30年代引入的一套用于研究函数定义、函数应用和递归的系统。
  - 2.函数式编程不是用函数来编程，也不是传统的面向过程编程。主旨在于将复杂的函数符合成简单的函数（计算理论，或者递归论，或者拉姆达演算）。运算过程尽量写成一系列嵌套的函数调用
  - 3.JavaScript是披着C外衣的lisp
  - 4.真正的火热是随着React的高阶函数而逐步升温



- - a.函数是一等公民。所谓“第一等公民”（first class）,指的是函数与其它数据类型一样，处于平等地位，可以赋值给其它变量，也可以作为参数，传入另一个函数，或者作为别的函数的返回值。
  - b.不可变量。在函数式编程中，我们通常理解的变量在函数式编程中也被函数代替了，在函数式编程中变量仅仅代表了某个表达式。这里所说的‘变量’是不能被修改的。所有的变量只能被赋一次初值。
  - c.map&reduce它们是最常用的函数式编程的方法。
  - - map方法
    - - 反回一个新数组，新数组中的元素为原始数组元素调用函数处理后的值
      - 按照原始数组元素顺序依次处理元素
      - 不会对空数组进行检测，不会改变原数组
      - array.map(function(currentValue,index,arr){},thsiValue),currentValue参数必须
    - reduce方法	
    - - 接受一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值
      - 可以作为一个高阶函数，用于函数的compose
      - 注：对于空数组是不会执行回调函数的。
      - array.reduce(function(total,currentValue,currentIndex,arr){},initialValue)
  - 函数是“第一等公民”
  - 只用表达式，不用语句
  - 没有副作用
  - 不修改状态
  - 引用透明（函数运行只靠参数）

2.函数式编程常用核心概念

- 纯函数
- - 对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态。

var s = [1,2,3,4];

s.slice(0,3);//[1,2,3]

s.slice(0,3) //[1,2,3]



//splice会改变原数组

s.splice(0,3);[1,2,3]

s.splice(0,3);//[4]

- - 优缺点
  - - 优点：纯函数不仅可以有效降低系统的复杂度，还有很多很棒的特性，比如可缓存性。

import _ from 'lodash';

var sin = _.memorize(x=>{Math.sin(x)});

//第一次计算的时候会稍微慢一点

var a sin(7);

//第二次有了缓存，速度极快

var b = sin(7); 

- - - 在不纯的版本中，checkAge不仅取决于age还有外部依赖的便令min，纯的checkAge把关键数字18硬编码在函数内部，扩展性比较差，柯里化优雅的函数式解决。

//不纯的

var min = 18;

var checkAge = age=>age>min;

//纯的

var checkAge = age=>age>18; 

- - - 纯度和幂等性
    - - 幂等性是指执行无数次后还具有相同的效果，同一个参数运行一次应该与连续运行两次的结果一致。幂等性在函数编程中与纯度相关，但又不一致。

Math.abs(Math.abs(-42));//42

- 函数的柯里化
- - 传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
  - 示例：柯里化上边的checkAge

var checkAge = min=>(age=>age>min);

vae checkAge18 = checkAge(18);

checkAge(20);

----------函数柯里化code-------------

//函数柯里化之前

function add(x,y){

return x+y;

}

add(1,2);//3



//柯里化之后

function addX(y){

return function (x){

return x+y;

}

}

addX(2)(1);//3

- - 优缺点
  - - 事实上柯里化是一种“预加载”函数的方法，通过传递较少的参数，得到一个已经记住了这些参数的新函数，某种意义上讲，这是一种对参数的‘缓存’，是一种非常高效的编写函数的方法

import {curry} from 'lodash';

var match = curry((reg,str)=>str.match(reg));

var filter = curry((f,arr)=>arr.filter(f));

var haveSpace = match(/\s+/g);

haveSpace("ffffff");

haveSpace('a b');

filter(haveSpace,['adsf','adfs adfs']);

filter(haveSpace)(['adsf','adfs adfs']);

- 函数组合
- - 纯函数以及如何把它写出的洋葱代码h(g(f(x)))柯里化，为了解决函数嵌套的问题，需要用到函数组合。
  - 代码示例

const compose = (f,g)=>(x=>f(g(x)));

var first = arr=>arr[0];

var reverse = arr =>arr.reverse();

var last = compose(first,reverse);

last([1,2,3,4,5]);//5

![2](img/2.png)

- Point Free
- - 把一些对象自带的方法转化成纯函数，不要命名转瞬即逝的中间变量。
  - 这个函数中，使用了str作为中间变量，但这个中间变量除了让代码变得长了一点以外是毫无意义的。
  - - const  f=str=>str.toUpperCase().split('');
  - 优缺点
  - - 下面的这种风格能够帮助我们减少不必要的命名，让代码保持简洁和通用。

var toUpperCase = word=>word.toUpperCase();

var split = x=>(str=>str.split(x));

var  f = compose(split('',toUpperCase));

f('ad adsf ads');

- 声明式代码与命令式代码
- - 命令式代码的意思就是，我们通过编写一条又一条指令去让计算机执行一些动作，这其中一半都会涉及到很多繁杂的细节。而声明式代码就要优雅很多了，我们通过写表达式的方式来声明我们想干什么，而不是通过一步步的指示。
  - 代码示例：

//命令式

let ceo = [];

for(var i = 0;i < compain.length;i++){

ceo.push(compain[i].ceo);

} 

//声明式

let ceo = compain.map(c=>c.ceo);

- - 优缺点
  - - 函数式编程的一个明显好处就是这种声明式的代码，对于无副作用的纯函数，我们完全可以不考虑函数内部是如何实现的。专注于编写业务代码。优化代码时，目光只需要集中在这些稳定坚固的函数内部即可。
    - 相反，不纯的函数式的代码会产生副作用或者依赖外部系统环境，使用它的时候总是要考虑这些不干净的副作用。在复杂的系统中，这对于程序员的心智来说是极大的负担。
- 惰性求值、惰性函数、惰性链
- - 在指令式语言中代码会按顺序执行，由于每个函数都有可能改动或者依赖于其它外部的状态，因此必须顺序执行。
  - 惰性求值代码示例

function ajax(){

if(XMLHttpRequest){

ajax = function(){

return new XMLHttpRequest();

}

}else{

ajax = function(){

return new ActiceXObject('Microsoft.XMLHTTP');

}

}

} 

- - 惰性链
  - - new LazyChain([1,2,3]).add().xx();后边可以点点，但是和链式调用不一样，简单说就是，链式调用，每一步都会执行有返回值，而惰性链每一步没有返回值，而是等到最后一步才返回具体的值

3.函数式编程深入

- 高阶函数
- - 函数当参数，把传入的函数做一个封装，然后返回这个封装函数，达到更高程度的抽象

//命令式

var add = function(a,b){

return a+ b;

} 

function math(fn,arr){

return fn(arr[0],arr[1]);

}

math(add[7,2]);//3

- - 它是一等公民
  - 它是一个函数作为参数
  - 以一个函数作为返回结果
- 尾调用优化
- - 指函数内部的最后一个动作是函数调用。该调用的返回值，直接返回给函数。函数调用自身，称为递归。如果尾调用自身，就是尾递归。递归需要保存大量的调用记录，很容易发生栈溢出错误，如果使用尾递归优化，将递归变为循环，那么只需要保存一个调用记录，这样就不会发生栈溢出错误了。

//不是尾递归无法优化

function fn(n){

if(n===1) return 1;

return n*fn(n-1);

} 

//是尾递归可以优化，es6强制使用尾递归

function fn(n,total){

if(n===1) return total;

return fn(n-1,n*total);

}

- - 普通递归
  - - 普通递归时，内存需要记录调用的堆栈所处的深度和位置信息，在最底层计算返回值，再根据记录的信息，跳回上一层级计算，然后再跳回更高一层，依次运行，直到最外层的调用函数。cpu计算和内存会消耗很多，而且当深度过深时，会出现堆栈溢出。

function sum(n){

n===1&&return 1;

return n+sum(n-1);

}

sum(5)

(5+sum(4))

(5+(4+sum(3)))

(5+(4+(3+sum(2))))

(5+(4+(3+(2+sum(1)))))

(5+(4+(3+(2+1))))

(5+(4+(3+3)))

(5+(4+6))

(5+10)

15

- - 尾递归
  - - 整个计算过程是线性的，调用一次sum(x,total)后，相关数据信息跟随进入下一个栈，不再放在堆栈上保存。当计算完值后，直接返回到最上层的sum(5,0).这能有效的防止堆栈溢出。
    - 在ECMAScript6，我们将迎来尾递归优化，通过尾递归优化，JavaScript代码在解释成机器码的时候，将会向while看齐，也就是说，同时拥有数学表达能力和while的效能。

function sum(x,total){

n===1&&return x + total;

return sum(x-1,x+total);

}

sum(5,0)

sum(4,5)

sum(3,9)

sum(2,12)

sum(1,14)

15

- 闭包
- - 如下例子，虽然外层的makePowerFn函数执行完毕，栈上的调用帧被释放，但是堆上的作用域并不释放，因此power依旧可以被powerFn函数访问，这样就形成了闭包。

function makePowerFn(power){

function powerFn(base){

return Math.pow(base,power);

}

return powerFn;

}

var square = makePowerFn(2);

square(3);//9

- 范畴与容器
- - 1.我们可以把‘范畴’想像成一个容器，里面包含两样东西。值、值的变形关系（也就是函数）。
  - 2.范畴论使用函数，表达范畴之间的关系
  - 3.伴随着范畴论的发展，就发展出一整套函数运算的方法。这套方法起初只用于数学运算，后来有人将它在计算机上实现了，就变成了今天的函数式编程。
  - 4.本质上，函数式编程只是范畴论的运算方法，跟数理逻辑、微积分、行列式是同一类东西，都是数学方法，只是碰巧它用来写程序。为什么函数式编程要求函数必须是纯的，不能有副作用？因为它是一种数学运算，原始目的就是求值，不做其它的事情，否则就无法满足函数运算法则了。

 

- - 1.函数不仅可以用于同一个范畴之中值得转换，还可以用于将一个范畴转成另一个范畴。这就涉及到了函子（Functor）
  - 2.函子是函数式编程里面最重要的数据类型，也是基本的运算单位和功能单位。它首先是一种范畴，也就是说，是一个容器，包含了值和变形关系。比较特殊的是，它的变形关系可以依次作用于每一个值，将当前容器变成另一个容器。

![image](img/image-7083604.png)

- 容器、Functor
- - $(‘’)返回的对象并不是一个原生的dom对象，而是对于原生对象的一种封装，这在某种意义上就是一个容器（但它并不函数式）
  - Functor(函子)是遵守一些特定规则的容器类型
  - 任何具有map方法的数据结构，都可以当做函子的实现。
  - Functor是一个对于函数调用的抽象，我们赋予容器自己去调用函数的能力。把东西装进一个容器，只留出一个接口map给容器外的函数，我们让容器自己来运行这个函数，这样容器就可以自由选择何时何地地如何操作这个函数，以至于拥有惰性求值、错误处理、异步调用等等非常牛掰的特性。
- 函子的代码实现

--------JavaScript写法-----------------

var  Container  =  function(x){

this.__value =  x;

}

//函数式编程一般约定，函子有一个of方法,用来实现new操作

Container.of =  x=>new Container(x);

//一般约定，函子的标志就是容器具有map方法。该方法将容器里面的额每一个值映射到另一个容器。

Container.prototype.map =  function(f){

return Container.of(f(this.__value));//返回一个新的容器

}

Container.of(3)

.map(x=>x+1)          //=>Container(4)

.map(x=>'result  is'+  x);      //=>Container('result is 4')

--------------es6写法----------------

class  Functor{

constructor(val){

this.val =  val;

}

map(f){

return new Functor(f(this.val))

}

}

//写法一，这样写在生成新函子的时候，用了new命令。不符合函数式编程，因为new命令式面向对象的标志。所以需要进行修改如下

(new Functor(2)).map(function  (two){

return two +2;

});

//Functor (4);

//写法二，修改

//函数式编程一般约定，函子有一个of方法，用来生成新的容器。

Functor.of =  function(val){

return new Functor(val);

};

Functor.of(2).map(function(two){

return two +  2;

})

//Functor(4)

- - 上面代码中，Functor是一个函子，它的map方法接受函数作为参数，然后返回一个新的函子，里面包含的值是被f处理过的f(this.val)。一般约定，函子的标志是拥有map方法。该方法将容器里面的每一个值，映射到另一个容器。
  - 上面的例子说明，函数式编程里面的运算，都是通过函子完成，即运算不直接针对值，而是针对这个值得容器----函子。函子本身具有对外接口（map方法），各种函数运算就是运算符，通过接口接入容器，引发容器里面的变形。因此，学习函数式编程，实际上就是学习函子的各种运算。由于可以把运算方法封装在函子里面，所以又衍生出各种不同类型的函子，有多少种运算，就有多少种函子。函数式编程就变成了运用不同的函子，解决实际问题。
- Maybe函子
- - 函子接受各种函数，处理容器内部的值。这里就有一个问题，容器内部的值可能是一个空值（比如null），而外部函数未必有处理空值的机制，如果传入空值，很可能就会出错。

//当传入空值的时候会报错，

Functor.of(null).map(function(s){

return  s.toUpperCase();//Cannot read property 'toUpperCase' of null

})

\------------------------------------

//通过Maybe函子进行处理如下,先对值进行一次判断

class  Maybe  extends Functor{

map(f){

return this.val?Maybe.of(f(this.val)):Maybe.of(null);//对传入的值进行判断

}

}

Maybe.of(null).map(function(s){

return s.toUpperCase();

})

//Maybe(null)//这样当传入空值的时候就不会报错了

\-------------------------------

var  Maybe  =  function(x){

this.__value =  x;

}

Maybe.of  =  function(){

return new Maybe(x);

}

Maybe.prototype.map =  function(f){

return this.isNothing()?Maybe.of(null):Maybe.of(f(this.__value));

}

//对判断空值再进行一层封装

Maybe.prototype.isNothing =  function(){

return (this.__value ===  null||  this.__value ===  undefined);

}

//新的容器我们称之为Maybe(原型来自Haskell)

- - 注：Haskell是函数式（一切通过函数调用来完成）、静态、隐式类型（类型由编译器检测，类型声明不是必须的）、惰性（除非必要，否则什么也不做）的语言。
- 错误处理、Either
- - 1.我们的容器能做的事情太少了，try/catch/throw并不是‘纯’的，因为它从外部接管了我们的函数，并且在这个函数出错时抛弃了它的返回值。
  - 2.Promise是可以调用catch来集中处理错误的
  - 3.事实上，Either并不只是用来处理错误的，它表示了逻辑或，范畴学里边的coproducts
  - 条件运算符if...else是最常见的运算之一，函数式编程里面，使用either函子表达。Either函子内部有两个值：左值（left）和右值（right）。右值是正常情况下使用的值，左值是右值不存在时使用的默认值。

class Either  extends Functor{

constructor(left,right){

this.left =  left;

this.right =  right;

}

map(f){

return this.right?Either.of(this.left,f(this.right)):Either.of(f(this.left),this.right);

}

} 

Either.of =  function(left,right){

return new Either(left,right);

}

var addOne =  function(x){

return x+1;

}

Either.of(5,6).map(addOne);//Either(5,7)

Either.of(1,null).map(addOne);//Either(2,null)

Either.of({address:'xxx'},currentUser.address).map(updatefield);

\----------------------------------------------------------------------

//代替try...catch

var  Left  =  function(x){

this.__value =  x;

}

var Right  =  function(x){

thsi.__value =  x;

}

Left.of  =  function(x){

return new Left(x);

}

Right.of =  function(x){

return new Right(x);

}

Left.prototype.map =  function(f){

return this;

}

Right.prototype.map =  function(f){

return Right.of(f(this.__value));

}

- - Left和Right唯一区别就在于map方法的实现，Right.map的行为和我们之前提到的map函数一样。但是Left.map就很不同了：它不会对容器做任何事情，只是简单的把这个容器拿进来又扔出去。这个特性意味着，Left可以用来传递一个错误消息。
  - 示例：

var   getAge =  user=>user.age?Right.of(user.age):Left.of("ERROR");

getAge({name:'wang',age:'21'}).map(age=>'age is'+  age);//Right('age is 21');

getAge({name:'wang'}).map(age=>'age is'+  age);//Left('ERROR')，因为age不存在执行Left

//left可以让调用链中任意一环的错误立刻返回到调用链的尾部，这给我们错误处理带来了很大的方便，再也不用一层又有一层的try/catch

- AP
- - AP因子
  - - 函数里面包含的值，完全可能是函数。我们可以想象这样一个情况，一个函子的值是数值，另一个函子的值是函数。
  - AP函子

class  Ap  extends Functor{

ap(F){

return Ap.of(this.val(F.val));

}

}

Ap.of(addTwo).ap(Function.of(2));

-----------------实例---------------------

function Functor(val){

this.__val = val;

}

Functor.of = function(val){

return new Functor(val);

}

Functor.prototype.map = function(fn){

return Functor.of(fn(this.__val));

}

function addTwo(x){

return x+2;

}

function Ap(val){

Functor.call(this._val)

}

Ap.of = function(val){

return new Ap(val);

}

var __proto = Object.create(Functor.prototype);

__proto.constructor = Ap.prototype.constructor;

Ap.prototype = __proto;

Ap.prototype.ap = function(F){

return new Ap.of(this.__val(F.__val));

}

const A = Functor.of(2);

const B = Functor.of(addTwo);//传入的值是一个函数

console.log(B.ap(A));//4

- IO
- - 1.真正的程序总要去接触肮脏的世界，在这个世界里，很多事情都是有副作用的或者依赖于外部环境的，比如下面这样：

function  readLocalStorage(){

return window.localStorage;

} 

- - - //这个函数显然不是纯函数，因为它依赖外部的window.localStorage这个对象，它的返回值会随着环境的变化而变化。为了让它纯起来，我们可以把它包裹在一个函数内部，延迟执行它,这样就变成了一个真正的纯函数。

function readLocalStorage(){

return function(){

return window.localStroage;

}

}

- - - 上面的写法，好像没什么用，我们只是（像大多数拖延症晚期患者那样），把讨厌做的事情暂时搁置了而已。为了能彻底解决这些讨厌的事情，我们需要一个加IO的新的Functor：

import  _from 'lodash';

var compose =  _.flowRight;

var IO  =  function(f){

this.__value =  f;

} 

IO.of  =  x =>new IO(_=>x);

IO.prototype.map =  function(f){

return new IO (compose(f,this.__value))

}

- - 2.IO跟前面那几个Functor不同的地方在于，它的__value是一个函数。它把不纯的操作（比如IO、网络请求、dom）包裹到一个函数内，从而延迟这个操作的执行。所以我们认为，IO包含的是被包裹的操作返回值。

var io_document = new IO(_=>window.document);

io_document.map(function(doc){return doc.title})

- - - 注意我们这里虽然感觉上返回了一个实际的值IO(document.title),但事实上只是一个对象：{__value:[Function]},它并没有执行，而是简单地把我们想要的操作存了起来，只有当我们在真的需要的时候，IO才会真的开始求值，这个特性我们称之为惰性求值。
    - 是的，我们依然需要某种方法让IO开始求值，并且把他们返回给我们。它可能因为map的调用链积累了很多很多不纯的操作，一旦开始求值，就可能会把本来很干净的程序给弄脏。但是去执行这些脏操作不行，我们把这些不纯的操作带来的复杂性和不可维护性推到了IO的调用者身上。
    - 下面我们来做点稍微复杂点的事情，编写一个函数，从当前的url中解析出对应的参数

import _ from 'lodash';

//先来介个基础的函数

//字符串

var split = _.curry((char,str)=>str.split(char));

//数组

var first = arr=>arr[0];

var last = arr=>arr[arr.length -1];

var filter = _.curry((f,arr)=>arr.filter(f));

//注意这里的x即可以是数组，也可以是functor

var map = _.curry((f,x)=>x.map(f));

//判断

var eq = _.curry((x,y)=>x===y);

//结合

var compose = _.flowRight;

var toPairs = compose(map(split('=')),split('&'));

//toPairs('a=1&b=2')

//[['a','1'],['b','2']]

var params  = compose(toPairs,last,split('?'));

//params('http://xxx.com?a=1&b=2')

//[['a','1'],['b','2']]

//这里会有些难懂，慢慢看

//1.首先，getParam是一个接受IO(url),返回一个新的接受key的函数

//2.我们先对url调用params函数，得到类似[['a','1'],['b','2']]这样的数组

//3.然后调用filter(compose(eq(key),first)),这是一个过滤器，过滤的，条件是compose(eq(key),first)为真，它的意思就是只留下首项为key的数组

//4.最后调用Maybe.of把它包装起来

//5.这一系列的调用是针对IO的，所以我们用map把这些调用封装起来

var getParam = url=>key=>map(compose(Maybe.of,filter(compose(eq(key),first)),params))(url);

//创建充满了洪荒之力的IO

var url = new IO(_=>window.location.href);

//最终的调用函数

var findParam = getParam(url);

//上面的代码都是很干净的纯函数，下面我们来对它求值，求值得过程是非纯的

//假设现在的url是<http://xxx.com?a=1&b=2>

//调用__value()来运行它

findParam('a').__value();//Maybe(['a','1'])

- - 3.IO其实也是惰性求值
  - 4.IO负责调用链积累的很多不纯的操作，带来的复杂性和不可维护性。

\-------------------------------------

import   _from 'lodash';

var compose =  _.flowRight;

class IO  extends Monad{

map(f){

return IO.of(compose(f,this.__value))

}

}

-----------IO函子-----------------------------

var  fs =  require('fs');

var rea的File  =  function(filename){

return new IO(function(){

return fs.readFileSync(filename,'utf-8');

})

}

readFile('./user.txt')

.flatMap(tail)

.flatMap(print)

//等同于

readFile('./user.txt')

.chain(tail)

.chain(prinit)

- 小结
- - 我们先后用到了Maybe、Either、IO这三种强大的Functor，在链式调用、惰性求值、错误捕获、输入输出中都发挥着巨大的作用，事实上Functor远不止这三种。
  - 1.如何处理嵌套的Functor呢？（Maybe(IO(42))）
  - 2.如何处理一个由非纯的或异步的操作序列呢？
- Monad
- - 1.Monad就是一种设计模式，表示将一个运算过程，通过函数拆解成互相连接的多个步骤。你只要提供下一步运算所需的函数，整个运算就会自动进行下去。
  - 2.Promise就是一种Monad
  - 3.Monad让我们避开了嵌套地狱，可以轻松的进行深度嵌套的函数式编程，比如IO和其它异步任务。

Maybe.of(Maybe.of({name:'wang',number:12})) ;

class Monad  extends Functor{

join(){

return this.val;

}

flatMap(f){

return this.map(f).join();

}

}

- - Monad函子的作用是，总是返回一个单层的函子。它有一个flatMap的方法，与map方法作用相同，唯一的区别是如果生成了一个嵌套函子，它会取出后者内部的值，保证返回的永远是一个单层的容器，不会出现嵌套的情况。如果函数返回的是一个函子，那么this.map(f)就会生成一个嵌套的函子。所以，join方法保证了flatMap方法总是返回一个单层的函子。这意味着嵌套的函子会被铺平（flatten）。

3.流行的几大函数式编程库

- RxJS(学习api)
- - RxJS从诞生以来一直都不温不火，但它函数响应式编程（Function Reactive  programming，FRP）的理念非常先进，虽然或许对于大部分应用环境来说，外部输入事件并不是太频繁，并不需要引入一个如此庞大的FRP体系，但我们也可以了解一下它有哪些优秀的特性。
  - 在RxJS中，所有的外部输入（用户输入、网络请求等等）都被视作一种事件流
  - 用户点击了按钮-->网络请求成功->用户输入键盘->某个定时事件发生->这种事件流特别适合处理游戏，如下

var  clicks =  Rx.Observable

.fromEvent(document,'click')

.bufferCount(2)

.subscribe(x=>console.log(x));//打印出前两次点击事件 

- - 响应式编程是继承自函数式编程，声明式的，不可变的，没有副作用的是函数式编程的三大护法。其中不可变武功最深。一直使用面向对象编程的我们，习惯了使用变量存储和追踪程序的状态。RxJS从函数式编程范式中借鉴了很多东西，比如链式调用，惰性求值等。
  - 在函数中与函数作用域之外的一切事物有交互就产生了副作用。比如读写文件，在控制台打印语句，修改页面元素css等。在RxJS中，把副作用问题推给了订阅者来解决。
- cycleJS
- - cyclejs是一个rxjs的框架，它是一个彻彻底底的FRP理念的框架和react一样支持virtula dom，jsx语法，但现在似乎没有看到大型的应用经验。
  - 本质上讲，它就是在rxjs的基础上加了对virtual dom、容器组件的支持，比如下面就是一个简单的开关按钮

function  main(sources){

const sinks =  {

DOM:sources.DOM.select('input').events('click')

.map(ev=>ev.target.checked)

.startWidth(false)

.map(toggled=>

<div>

<input type="check"  />toggle me

<p>{toggled?'ON':'off'}</p>

</div>

)

};

return sinks;

}

const drivers =  {

DOM:makeDOMDrive("#app");

}

run(main,drives);

- LodashJS、lazy（惰性求值）
- - lodash是一个具有一致接口、模块化、高性能等特性的javascript工具库，是udnerscorejs的fork，其最初目标也是“一致的浏览器行为，并改善性能”。
  - lodash采用延迟计算，意味着我们的链式方法在显示或者隐式的value()调用之前是不会执行的，因此lodash可以进行shortcut（捷径），fusion(融合)这样的优化，通过合并链式大大降低迭代的次数，从而大大提升其执行性能。
  - 就如同jquery在全部函数前加全局的$一样，lodash使用全局的_来提供对工具的快速访问。

var  abc =  function(a,b,c){

return [a,b,c];

}

var curried =  _.curry(abc);

curried(1)(2)(3);

\------------------------------

function square(n){

return n*n;

}

var addSquare =  _.flowRight(square,_.add);

addSquare(1,2);//9

- underscoreJS
- - underscorejs是一个javaScript工具库，它提供了一整套函数式编程的实用功能，但是没有扩展任何javaScript内置对象。他解决了这个问题：‘如果我们面对一个空白的html页面，并希望立即开始工作，我们需要什么？’它弥补了jquery没有实现功能，同时又是backbone必不可少的部分。
  - undescore提供了100多个函数，包括常用的：map,filter,invoke,当然还有更多专业的辅助函数，如：函数绑定、javaScript模板功能、创建快速索引、强类型相等测试等等。
- ramdajs
- - ramda是一个非常优秀的额sj工具库，跟同类比更函数式，主要体现在以下几个原则
  - - 1.ramda里面的提供的函数全部都是curry的，意味着函数没有默认参数可选参数从而减轻认知函数的难度。
    - 2.radma推崇pointfree简单的说是使用简单函数组合实现一个复杂功能，而不是单独写一个函数操作临时变量。
    - 3.ramda有个非常好用的参数占位符R._大大减轻了函数在pointfree过程中参数位置的问题。
    - 相比undescore/lodash感觉要干净很多



任务看下rxjs官方的api，把underscorejs（有点不是纯粹的函数式编程，有点老了，不过代码非常有学习的意义，学函数式编程的思想，js编程的技巧）的源码看一下 



lodashjs不推荐读源码，比较复杂，学lodashjs，

4.函数式编程的实际应用场景

- 易调试、热部署、并发
- - 1.函数式编程中的每个符号都是const的，于是没有什么函数会有副作用。谁也不能在运行时候修改任何东西，也没有函数可以修改再它的作用域之外修改什么值给其它函数继续使用。这意味着决定函数执行结果的唯一因素就是它的返回值，而且影响其返回值的唯一因素就是参数。
  - 2.函数式编程不需要考虑死锁（deadlock），因为它不修改变量，所以根本不存在锁线程问题。不必担心一个线程的数据，被另一个线程修改，所以可以很放心的把工作分摊到多个线程，部署‘并发编程’（concurrency）
  - 3.函数式编程中所有状态就是传给函数的参数，而参数都是储存在栈上的。这一特性让软件的热部署变得十分简单。只要比较一下正在运行的代码以及新的代码获得一个diff，然后用这个diff更新现有的代码，新代码的热部署就完成了。

5.单元测试

- 严格函数式编程的每一个符号都是对直接量或者表达式结果的引用，没有函数产生副作用。因为从未在某个地方修改过值，也没有函数修改过在其作用域之外的量并被其它函数使用（如类成员，或全局变量）。这意味着函数求值的结果只是返回值，而唯一影响其返回值的就是函数的参数。
- 这是单元测试者的梦中仙境（wet  dream）。对被测试程序中的每个函数你只需在意其参数，而不必考虑函数调用顺序，不用谨慎地设置外部状态。所有要做的就是传递代表了边际情况的参数。如果程序中的每个函数都通过了单元测试，你就对这个软件的质量有了相当的自信。而命令式编程就不能这样乐观了，在java或c++中只检查函数的返回值还不够----我们还必须验证这个函数可能修改了的外部状态。

6.总结与补充

- 函数式编程不应该被视为灵丹妙药。相反，它应该被视为我们现有工具箱的一个很自然的补充---它带来了更高的可组合性，灵活性以及容错性。现在的javascript库已经开始尝试拥抱函数式编程的概念以获取这些优势。redux作为一种flux的变种实现，核心理念也是状态机的函数式编程。
- 函数对于外部状态的依赖是造成系统复杂性大大提高的主要原因
- 要让函数尽可能的纯净
- 我们可能没有机会在生成环境中自己去实现这样的玩具级Functor，但通过了解它们的特性会让你产生对于函数式编程的意识。
- 软件工程上讲，‘没有银弹’，函数式编程同样也不是万能的，它与烂大街的oop一样，只是一种编程范式而已。很多实际应用中是很难用函数式去表达的，选择oop亦或是其它编程式或许更简单。但我们要注意到函数式编程的核心理念，如果说oop降低复杂度是靠良好的额封装、继承、多态以及接口定义的话，那么函数式编程就是通过纯函数以及他们的组合、柯里化、Functor等技术来降低系统复杂度，而react，rxjs，cyclejs正式这种理念的代言。让门一起拥抱函数式编程，打开程序的大门。

## 二、javaScript与QA测试工程师

TDD(测试驱动开发)先测再写，要求比较高

BDD（行为驱动开发）先写再测，一般先写再测

1.unit单元自动化测试（数据处理层）

- 简介：

unit测试时把代码看成是一个组件。从而实现每一个组件的单独测试，测试内容主要是组件内每一个函数的返回结果是不是和期望值一样

- 安装与代码

安装karma

初始化karma  init

选择jasmine 断言库，可以选择其它的

PhantomJS    已经不行了，已经停止维护了,现在用的是rize，新的无头浏览器，非常简洁

测试就不用再写es6了，要不还得进行编译

配置karma.conf.js

```js
files:[

"./test/unit/**/*.js",

]

singleRun:true
```



配置package.json

`"unit":"karma start"`

npm run unit

报错，缺少jasmine-core，安装

安装karma-jasmine，karma-phantomjs-launcher

再运行npm  run unit测试通过

生成覆盖率检查测试报告   安装karma-coverage

代码覆盖率是指代码中每一个函数的每一种情况的测试情况

配置karma.conf.js

```js
reporters:['process','coverage']

preprocessors:{

'./test/unit/**/*.js':['coverage']

}		
```

2.UI自动化测试 （GUI界面层）

phantomcss测html界面，不建议使用这个，建议使用backstop

安装backstopjs,之前是backstop，

npm  install -g backstopjs      

装本地也可以，装本地就在package.json中进行调用,自带的就不用加run，不是的就要加，npm  run unit

//安装有问题，就用yarn  add backstopjs global     

backstop   init  初始化，生成backstop.json配置文件

运行backstop  test 

在配置文件中可以配置要测试的屏幕尺寸

url要测试的项目地址：

在生成的backstop_data文件下新建backstop_reference，在bitmaps_reference文件下放入要测试对比的设计稿pnd图片，文字要和bitmaps_test中的文件名一样。

再运行backstop test ，提示没有找到portfinder，安装，再运行成功

3.自动化接口测试

- 什么是接口测试，接口测试是测试系统组件间接口的一种测试。接口测试主要用于检测外部系统与系统之间以及各个子系统之间的交互点。测试的重点是要检查数据的交换，传递和控制管理过程，以及系统间的相互逻辑依赖关系等。
- 安装与代码

用express先起个服务，安装express

package.json配置启动命令，因为express安装到本地了，“service”："node ./service/app.js"

服务启动之后，开始写接口测试代码

用supertest进行一层代理

测试完一定要进行退出，done()

用macha进行接口异步测试，安装mocha

将mocha测试集成到js，用node启动，用cli就不用node启动了

mochaawesome  生成报表

在根目录新建mocharunner.js

最后一定要进程退出，process.exit();

4.e2e测试    端对端测试

- 简介
  - 站在用户的角度的测试，我们的程序看成是一个黑盒子，我不懂你内部是怎么实现的，我只负责打开浏览器，把测试内容在页面上输入一遍，看是不是我想要得到的结果。
- 安装与代码

selenium-webdirver

下载geckodrier,放到项目根目录



nightwatch在vue中使用的，不太好用

f2etest可以录制，阿里出的，可以测试ie6什么的，比价不错

rize只支持chrome内核

5.性能测试

benchmarkjs    基于方差的测试，谁的代码快

## 三、JavaSCript与QA测试工程师补课

## 四、第一周实战

## 五、第一周试卷讲解

