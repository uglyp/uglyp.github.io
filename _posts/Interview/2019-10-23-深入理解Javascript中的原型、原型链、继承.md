---
layout: blog
istop: false
title: "深入理解Javascript中的原型、原型链、继承"
background-image: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=493146188,1000916900&fm=15&gp=0.jpg
date:  2019-10-23 12:45:56
category: js
description: <p>深入理解Javascript中的原型、原型链、继承</p>
tags:
- js
---

# **构造函数,原型,实例三者的关系**

## 构造函数:

构造函数是创建对象的一种常用方式, 其他创建对象的方式还包括工厂模式, 原型模式, 对象字面量等.我们来看一个简单的构造函数:

```javascript

function Product (name, role){ //构造函数的命名约定第一个字母使用大写的形式!
    this.name = name;
    this.role = role;
}
```
( 1 ) 每一个构造函数都有一个`prototype`属性,我们可以在`Chorme`控制台中打印出`Product.prototype`属性.

![](http://static.zybuluo.com/leeahui424/ppntgpldmqim18jbqfkwy7ll/image_1bluplrdgp0qg361aquq0rj5v9.png)

( 2 ) 通过控制台打印出的结果可以发现,`Product.prototype`属性其实是一个指针,指向一个对象, 该对象拥有一个`constructor`属性.( `__proto__`属性是浏览器引入的非标准属性,方便开发者调试用的,可以不予理会!)

( 3 ) 所以,构造函数中的`prototype`属性指向一个对象, 该对象就是我们接下来要说的原型

## 原型

《Javascript高级程序设计》中是这样描述原型这个概念的:

无论什么时候, 只要创建一个新函数, 就会根据一组特定的规则为该函数创建一个`prototype`属性, 这个属性指向函数的原型对象. 在默认情况下, 所有原型对象都会自动获得一个`constructor()`构造函数属性,这个属性包含一个指向`prototype`属性所在函数的指针.

我相信看完这句话的你是懵逼的,下面我来解释一下这句话:

( 1 ) **构造函数**的`prototype`属性所指向的对象我们将其称为**原型**
( 2 ) **原型**这个对象中,有一个`constructor`属性又指回其构造函数本身.
( 3 ) 下面的图二,可以更清晰的弄明白两者之间的关系.

![](http://static.zybuluo.com/leeahui424/nnk52v1u9sqf8tugimf8hqbr/image_1blurjn2aa1bauiiq8mrb1p8tm.png)

## 实例

通过构造函数创建对象的过程我们称之为**实例化**，创建出来的对象称为**实例**．现在我们通过我们的`Product`这个构造函数创建一个实例(对象).

```javascript
function Product (name, role){
    this.name = name;
    this.role = role;
}

var p1 = new Product('apple', 'fruit');

```

( 1 ) 现在我们来为原型添加一个方法

```javascript
Product.prototype.greet = function(){
    console.log('我是一个很甜的'+ this.name +', 买我!')
}

```

( 2 ) 在实例中调用该方法

```javascript
p1.greet();  //我是一个很甜的apple, 买我!
```

可以看到,我们在原型中添加的方法,同时实例是可以使用的. 所以,我们可以得出以下结论:

> **原型里面的所有属性和方法会连接到其对应的构造函数的实例上 !**

( 3 ) 构造/实例/原型三角形

通过下面的构造/实例/原型三角形可以更好的理解三者之间的关系:

![](http://static.zybuluo.com/leeahui424/k3uhaltri7g4y9o8wd37dr0p/image_1blut26tt1ghb12jo1ir8gpm1n9913.png)

## 结论

- **站在构造函数角度看**: 函数名.prototype这个对象是构造函数的原型属性
- **站在实例对象角度看**: 函数名.prototype这个对象是实例对象的原型对象
- **原型式继承**: 原型对象的属性和方法可以直接被实例对象所访问.

# **原型链**


## 理解原型链

从第一部分的结论中我们知道了,实例对象可以直接访问原型对象的属性和方法. 其实原型对象本质上也是一个对象,它其实是由new Object()实例化出来的.该原型对象也可以访问Object.prototype的所有属性和方法.这样其实就构成了原型链这一个重要的概念.

## 代码分析

下面我们来分析以下一段代码

```javascript

//创建Product构造函数
function Product (){
    this.name = 'apple';
}
//给Product构造函数的原型增加方法
Product.prototype.greet = function(){
    console.log('我是一个很甜的'+ this.name +', 买我!');
}

//创建SalesProduct构造函数
function SalesProduct (){
    this.name = 'bad apple';
}

//将Product的实例对象直接赋给SalesProduct的原型
SalesProduct.prototype = new Product();

//给SalesProduct的原型一个方法
SalesProduct.prototype.anotherGreet = function(){
    console.log('我是一个' + this.name + ',不要买我!');
}

//实例化SalesProduct
var p2 = new SalesProduct(); 
p2.greet(); //我是一个很甜的bad apple, 买我!

```

( 1 ) 在上面的代码中,我们没有使用`SalesProduc`t默认提供的原型,而是将其换了一个新的原型.该新**原型**是`Product`的实例.
( 2 ) 所以,该新**原型**不仅作为`Product`的**实例**拥有其全部属性和方法, 其内部还拥有一个指针,指向`Product.prototype`.我们可以从调用`p2.greet()`的结果可以看出.
( 3 ) 所以,可以得出以下的链条` p2 => SalesProduct.prototype => Product.prototype => Object.prototype`
( 4 ) 上面的这一个链条就形成了**原型链**.值得注意的是,`Product.prototype`最后还指向了`Object.prototype`,这也是所有的自定义对象都会具有`toString()`,`hasOwnProperty()`等方法的原因.

## 重要结论

( 1 ) **原型的顶端**: `Object.prototype`, 任何一个默认的内置的函数的原型都继承自`Object.prototype`.
( 2 ) **原型链** : Js的对象结构中出现的指向`Object.prototype`的一系列原型对象,我们称之为原型链.
( 3 ) **属性搜索原则** : 在访问对象的属性和方法时, 会在当前对象中查找, 如果没有找到, 会一直沿着原型链上的原型对象向上查找, 直到找到`Object.prototype`为止
( 4 ) **写入原则** : 如果给对象设置属性和方法, 都是在当前对象上设置.

# **实现继承的其他方法**

原型链继承是`Javascript`中实现继承的主要方式,但是原型链的继承存在一个问题,就是当对象有一个引用类型的属性时,该引用类型会被其后的所有实例所共享.下面的代码可以说明这一个问题.

```javascript
function TypeOne(){
        this.colors = ['red', 'blue', 'green'];
    }

    function TypeTwo(){}

    TypeTwo.prototype = new TypeOne();

    var t1 = new TypeTwo();
    t1.colors.push('black');
    console.log(t1.colors);  //["red", "blue", "green", "black"]

    var t2 = new TypeTwo();
    console.log(t2.colors); //["red", "blue", "green", "black"]   
```

可以看,在t1这个实例中进行的`colors`这个属性的更改在t2这个实例中反映了出来.也就是说,所有的实例对象共享同一`colors`属性.

基于原型链继承的这一缺点,我们需要通过其他方式实现继承,下面来介绍几个常用的方法.

## 借用构造函数
在解决原型中包含引用类型值所带来问题的过程中, 开发人员开始使用一种叫借用构造函数的技术. 这种技术的基本思想相当简单, 即在子类型构造函数的内部调用超类型构造函数. 函数只不过是在特定环境中执行代码的对象, 因此, 通过使用`apply()`和`call()`方法可以在新创建的对象上执行构造函数,我们通过以下代码来解决上面的问题:

```javascript
function TypeOne(){
    this.colors = ['red', 'blue', 'green'];
}

function TypeTwo(){
    TypeOne.call(this);
}

TypeTwo.prototype = new TypeOne();

var t1 = new TypeTwo();
t1.colors.push('black');
console.log(t1.colors);  //["red", "blue", "green", "black"]

var t2 = new TypeTwo();
console.log(t2.colors); //["red", "blue", "green"]
```

## 混入继承
我们可以通过一个克隆函数`extend()`,将一组对象中的所有属性和方法克隆到一个对象中,这种实现继承的方法我们,我们将其称为混入继承.在jQuery的源码中,大量的采用了该方法.该克隆函数代码如下:

```javascript
function extend (des, objs){
    for(var i = 0; i < objs.length; i++){
        for(var key in objs[i]){
            if(objs[i].hasOwnProperty(key)){
                des[key] = objs[i][key];
            }
        }
    }
}
```

下面,我们通过该克隆函数实现三个对象中属性和方法继承到一个对象上:

```javascript
var o1 = {
    name : 'Lee_Tanghui'
}

var o2 = {
    sayHi: function (){
        console.log('你好,我是' + this.name);
    }
}

var o3 = {
    introduce: function (){
        console.log('我来自重庆');
    }
}

var obj = {};


//调用该方法实现让obj继承o1,o2,o3
extend(obj, [o1, o2, o3]);

console.log(obj.name); //Lee_Tanghui
obj.sayHi(); //你好,我是Lee_Tanghui
obj.introduce(); //我来自重庆
```

从上面的结果可以看到,obj对象继承了来o1,o2,o3的所有方法和属性.

## 组合继承

组合继承,指的是将原型链和借用构造函数的技术组合到一块. 从而发挥二者之长的一种继承模式. 其背后的思路是使用原型链实现对原型属性和方法的继承, 而通过借用构造函数来实现对实例属性的继承. 这样, 既通过在原型上定义方法实现了函数复用, 又能保证每个实例都有它自己的属性.我们可以通过以下代码来了解组合继承:

```javascript
function TypeOne(name){
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

TypeOne.prototype.sayName = function(){
    console.log(this.name);
}

function TypeTwo(name, age){
    TypeOne.call(this, name); //通过构造函数实现对实例属性的继承
    this.age = age;
}

TypeTwo.prototype = new TypeOne(); //通过原型链实现对原型属性和方法的继承


TypeTwo.prototype.sayAge = function() {
    console.log(this.age);
}

var t1 = new TypeTwo('Lee_Tanghui', 23);
t1.colors.push('black');
console.log(t1.colors);  //["red", "blue", "green", "black"]
t1.sayName(); //Lee_Tanghui
t1.sayAge(); //23

var t2 = new TypeTwo('Joe', 24);
console.log(t2.colors); //["red", "blue", "green"]
t2.sayName(); //Joe
t2.sayAge(); //24
```

## 原型式继承

原型式继承是借助一个函数,通过传递原型对象作为参数,从而创建新对象的方式实现继承.该函数由道格拉斯提出,代码源码如下:

```javascript
function object(o) {
    function F(){};
    F.prototype = o;
    return new F();
}
```

在ECMAScript 5中新增加了`Object.create()`方法规范化了原型式继承.我们可以通过该方法实现将一下对象

```javascript
var person = {
        name : 'joe',
        age : 24
    }
```

变为

```javascript
[{key: "name", value: "joe" }, {key: "age", value: 24}]的数组的形式
```

代码实现如下

```javascript
var person = {
    name : 'joe',
    age : 24,
}

//创建构造函数,用来排列键值对
function SortedList ( obj ) {
    for ( var key in obj ) {
        this.push({
            key : key,
            value : obj[ key ]
        })
    }
}

//调用Object.create()方法,继承数组的所有属性方法
SortedList.prototype = Object.create( Array.prototype );

//构造函数的实例化
var list = new SortedList( person );

console.log( list ); //[{key: "name", value: "joe" }, {key: "age", value: 24}]

```


链接：https://www.jianshu.com/p/7581af2b450e
