# JSLearning

基于JavaScript高级程序设计第四版的学习笔记

## 学习疑问点

* p46 为什么Object.getOwnPropertyNames()不返回某些属性？
* p47 常用内置符号（还没看
* p132 单例内置对象Math（还没看
* p142 检测数组（不理解
* p165/174 的SameValueZero（不懂
* p168 的WeakMap（不理解
* p174 的add()和delete()是幂等的？什么是幂等？
* p175 forEach的第二个参数是干什么的
* p176 定义正式集合操作有空写一遍
* p178 的WeakSet不理解
* p191 迭代器，怎么样才是可关闭的？
* p193 生成器对象默认的迭代器是自引用的该怎么理解？
* p196 yeild关键字同时用于输入和输出的例子（不理解
* p199 使用yield*实现递归算法（不理解最后一次递归的细节
* p228 不理解setPrototypeOf()
* string应该是类数组类型吧，为什么不能使用数组的shift方法呢。-----哦哦string不是类数组对象，它是原始值对象，是不可修改的，所以用不了shift
* p245-247 原型式继承、寄生式继承和寄生式组合继承（不是很理解
* p251 类构造函数不返回this对象的情况（不懂
* p253 中两种类的构造函数的使用instanceof的返回为什么不同
* p257 类本身上的生成器和迭代器
* p266 第9章代理与反射先跳过不看
* p301 在严格模式下访问arguments.caller并没有报错，跟书上说不一样
* p344 串行期约合成（不懂
* p345 期约取消那个示例代码，每次点击取消都会添加取消按钮的监听器，导致会有很多重复的监听器，如何删除？
* p353 停止和恢复执行中的示例代码，我执行的结果跟书上不太一样。是为什么？
* p359 栈追踪与内存管理？？？
* p360 手写Promise源码，我本来想的是通过Object.defineProperty()设置PromiseState属性的特性configurable为false来达到状态一旦改变后就不会再改变的特性，但网上找的视频是通过if判断的，是否自己想的方法要好一点。但是在严格模式下是会报错的。
* p364 到底如何确定窗口大小？
* p382 第13章客户端检测先跳过不看

## 第2章 HTML中的JavaScript

### 2.1 \<script\>标签

#### 2.1.1 标签位置

* 使用了src属性的\<script\>元素会忽略行内代码

* 浏览器在解析资源时，会向src属性指向的路径发送一个GET请求，这个初始请求不受浏览器同源策略限制。

* \<script\>标签的integeity属性是防范引用文件被恶意替换问题的武器之一，但浏览器不一定支持

* 现代Web应用程序通常将所有JavaScript引用放在\</body\>前的位置，使页面在处理js代码之前完全渲染完页面。

#### 2.1.3 异步执行脚本

* 因为给脚本添加async属性就是告诉浏览器不必等待脚本下载完和执行完后再加载页面，也因此异步脚本不应该在加载器期间修改DOM

#### 2.1.4 动态加载脚本

* JavaScrpit可以使用DOM API向DOM中动态添加script元素加载指定脚本。因为这种方法获取的资源对于浏览器预加载器使不可见的，可以在头部显示声明将会动态加载的脚本资源

```html
<link rel="preload" href="xxxx.js">
```

## 第3章 语言基础

### 3.1 语法

>最佳实践是始终在控制语句中使用代码块，即使执行语句只有一句

### 3.3 变量

>不推荐通过省略var操作符定义全局变量

#### 3.1.1 var关键字

* hoist声明提升（var会提升，let不会）

```js
/*
*   hoist
 */
function foo() {
    console.log(age);
    var age = 16;
};
foo();

//等价于
function foo() {
    var age;
    console.log(age);
    age = 16;
};
foo();
```

#### 3.3.2 let声明

* for循环中的let声明

```js
/*
*   迭代变量
 */
for (var i = 0; i < 5; ++i) {
    // console.log(i);
    setTimeout(() => {
        console.log(i);
    });
}
for (let i = 0; i < 5; ++i) {
    // console.log(i);
    setTimeout(() => {
        console.log(i);
    });
}
```

#### 3.3.3 const声明

```js
/*
*   for-in和for-of循环中用const特别有意义
 */
for (const key in {a: 1, b: 2}) {
    console.log(key);
}
for (const value of [1, 2, 3, 4, 5]) {
    console.log(value);
}
```

>最佳实践是限制自己只用let和const，优先使用const

### 3.4 数据类型

#### 3.4.2 Undefined类型

* 使用未声明的变量会报错，对未声明的变量，只能执行一个有用的操作，typeof

```js
let message;
if(message){
    //这个块不会执行
    //因为值为undefined的undefined类型变量在if判断时会被Boolean()转化成false进行判断
}else{
    //这个块会执行
}
if(!message){
    //这个块会执行
}
if(age){//未声明age
    //这里会报错
}
```

>建议声明变量的同时进行初始化,同时可以通过初始化标明变量类型，比如

```js
let found=falae,
    count=-1,
    name='',
    person=null;
```

#### 3.4.3 Null类型

* 逻辑上讲，null值表示一个空对象指针

>在定义将来要保存对象值的变量时，建议使用null来初始化

* undefined值是由null值派生来的

ECMA-262将他们定义为表面上相等，(==)操作符比较始终返回true

#### 3.4.5 Number类型

* 浮点值，在算数运算中没有整数精准，0.1+0.2=0.30000000000000004,原因是因为使用了IEEE754数值

>永远不要测试某个特定的浮点值

* NaN，任何涉及NaN的操作始终返回NaN，NaN不等于NaN在内的任何值

```js
console.log(0/0);//NaN
console.log(-0/+0);//NaN
console.log(5/0);//Infinity
console.log(5/-0);//-Infinity
console.log(NaN==NaN);//false
```

* 数值转换

>通常在需要得到整数时优先使用parseInt()函数

>为了避免出错，建议始终传给parseInt()第二个参数，表示转化成几进制数

#### 3.4.6 String类型

* 模板字面量,,一个特点就是支持字符串插值

```js
let value = 5;
let exponent = 'second';
console.log(value + ' to the ' + exponent + ' power is ' + (value * value));

let interpolatedTemplateLiteral = `${value} to the ${exponent} power is ${value * value}`;
console.log(interpolatedTemplateLiteral)
```

```js
console.log(`Hello, ${`World`}!`);

let foo = {toString: () => 'World', inString: () => 'Word'};//省略掉了{}和return的箭头函数
console.log(`Hello, ${foo}!`);
console.log(`Hello, ${foo.toString()}!`);
console.log('Hello, ' + foo.inString() + '!');

function capitalize(word) {
    return `${word[0].toUpperCase()}${word.slice(1)}`
}
console.log(`${capitalize('hello')}, ${capitalize('world')}!`)

let value = '';
function append() {
    value = `${value}abc`
    console.log(value);
}
append();
append();
append();
```

* 模板字面量标签函数

```js
let a = 6;
let b = 9;

function simpleTag(strings, ...expressions) {
    console.log(strings);
    for (const expression of expressions) {
        console.log(expression);
    }
}

simpleTag(`${a} + ${b} = ${a + b}`)//作为一个参数将6 + 9 = 15传入
simpleTag`${a} + ${b} = ${a + b}`//分别传入了4个参数
simpleTag(['', ' + ', ' = ', ''], a, b, a + b)

function zipTag(strings, ...expressions) {
    return strings[0] + expressions.map((e, i) => `${e}${strings[i + 1]}`).join('');
}
console.log(zipTag`${a} + ${b} = ${a + b}`)
console.log(zipTag(['', ' + ', ' = ', ''], a, b, a + b))
```

#### 3.4.7 Symbol类型

* 分为全局与普通

```js
let symbol1=Symbol.for('foo');//创建全局符号
let symbol2=Symbol.for('foo');//重用已有符号
console.log(symbol1===symbol2);//true

let symbol3=Symbol('fo');//创建普通符号
let symbol4=Symbol('fo');
console.log(symbol3===symbol4);//false
console.log(symbol1===symbol3);//false
```

* 使用符号作为属性

```js
let s1 = Symbol.for('foo'),
    s2 = Symbol.for('bar'),
    s3 = Symbol.for('baz');
let o = {
    name:'hxh',
    age:12,
    [s1]: 'foo val',
    [s3]: 'baz val',
};
Object.defineProperty(o, s2, {value: 'bar val'});
// Object.defineProperties(o, {
//     [s2]:{value:'bar val',writable:true}
// });
console.log(o);
console.log(Object.getOwnPropertyNames(o));
console.log(Object.getOwnPropertySymbols(o));
console.log(Object.getOwnPropertyDescriptor(o,s1));
console.log(Object.getOwnPropertyDescriptors(o));
console.log(Reflect.ownKeys(o));
```

>有一个问题，为什么我Object.defineProperty()定义的属性，明明定义进去了，直接console.log()对象却不显示

## 第4章 变量、作用域与内存

### 4.1.2 传递参数

```js
function setName(obj){//这个obj为函数内的局部变量，但指向与person相同
    obj.name='Nicholas';//首先将本来person指向的内存中的对象添加了一个name值为'Nicholas'
    obj=new Object();//然后又将obj指向一个新的对象
    obj.name='Greg';//这个变量与其指向的本地对象在函数结束时就被销毁了。就算不销毁person也不指向它
}
let person=new Object();
setName(person);//person始终是指向当处分配的内存中的对象
console.log(person);
```

### 4.2 执行上下文与作用域

#### 4.2.2 变量声明

* 向未声明的 JavaScript 变量分配值
    * 如果您把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性。
        ```js
        carname="Volvo";
        ```
        将声明 window 的一个属性 carname。
        非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。
    * 如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。
        以下实例中 carName 在函数内，但是为全局变量。
        ```js
        // 此处可调用 carName 变量
        function myFunction() {
            carName = "Volvo";
            // 此处可调用 carName 变量
        }
        ```

### 4.3.4 内存管理

* 解除引用——优化内存占用的最佳手段就是保证执行代码时只保存必要的数据。如果数据不再必要，就把它设置为null，从而释放其引用。（适合全局对象和全局对象的属性）

#### 3.内存泄露

* 意外声明全局变量是最常见但也最容易修复的内存泄露问题

* 定时器也可能会悄悄地导致内存泄露

* 使用JavaScript闭包很容易在不知不觉间造成内存泄露

## 第5章 基本引用类型

### 5.3.3 String

#### 2.normalize()方法

>用于将字符规范为一致的格式，因为有些Unicode字符可以有多种编码方式，不同编码方式虽然表现出来是一样的，但是判断===时时不同的

## 第6章 集合引用类型

### 6.3 定型数组

## 第7章 迭代器与生成器

### 7.2 迭代器模式

#### 7.2.1 可迭代协议

* 如果对象原型链上的父类实现了Iterable接口，那这个对象也就实现了这个接口。

#### 7.2.2 迭代器协议

* 只要迭代器到达done:true状态，后续调用next()就一直返回同样的值了

* 每个迭代器都表示可迭代对象的一次有序遍历。不同迭代器的实例之间没有联系

* 如果可迭代对象在迭代期间被修改了，那么迭代器也会反应相应的变化

#### 7.2.4 提前终止迭代器

* 如果迭代器没有关闭，则还可以继续从上次离开的地方继续迭代。

### 7.3 生成器

#### 7.3.1 生成器基础

* 箭头函数不能用来定义生成器函数

* 生成器对象实现了Iterable接口，它们默认的迭代器是自引用的

#### 7.3.2 通过yield中断执行

* yeild关键字必须直接位于生成器函数定义中，出现在嵌套的非生成器函数中会抛出语法错误。

```js
// 在生成器function* 函数中，当实例第一次next()时，会运行到yeild之前停止，也就是只执行yeild之前的内容，因为第一次调用是为了开始执行生成器函数。然后后面每一次执行next()，都是运行到下一个yeild之前，也就是每次执行一个yeild，直到没有返回done:true
function* generatorFn(initial){
    console.log(initial);
    console.log(yield);
    console.log(yield);
}
let generatorObject=generatorFn('foo');
generatorObject.next('bar');//foo
generatorObject.next('baz');//baz
generatorObject.next('qux');//qux
```

```js
// yield*关联普通迭代器
function* generatorFn(){
    console.log('iter value:',yield* [1,2,3]);
}

for(const x of generatorFn()){
    console.log('value',x);
}
// value 1
// value 2
// value 3
// iter value: undefined

// 一开始每次遇到yield*都返回后面可迭代对象中的一个值，等到可迭代对象遍历完了，就可以确定yield*的值了（由于可迭代对象没有
// 返回值，所以是undefined），然后执行卡在的行语句。

// yield*关联生成器函数

function* innerGeneratorFn(){
    yield 'foo';
    return 'bar';
}

function* outerGeneratorFn(genObj){
    console.log('iter value',yield* innerGeneratorFn());
}

for(const x of outerGeneratorFn()){
    console.log('value:',x);
}
// value: foo
// iter value bar
```

#### 7.3.3 生成器作为默认迭代器

* 因为生成器对象实现了Iterable接口，而且生成器函数和默认迭代器被调用之后都产生迭代器。所以生成器格外适合作为默认迭代器。

#### 7.3.4 提前终止生成器

* 所有生成器对象都有return()方法，只要通过它进入关闭状态，就无法恢复了。

* for-of循环等内置语言结构胡忽略状态为done:true的可迭代对象。

* throw()方法会在暂停的时候将一个提供的错误注入到生成器对象中，如果错误未被处理，生成器就关闭了。

* 假如生成器函数内部处理了这个错误，生成器就不会关闭。错误处理会跳过对应的yield。

* 如果生成器对象还没开始执行，那么调用throw()抛出的错误不会在函数内部被捕获，因为这相当于在函数块外部抛出错误。

## 第8章 对象、类与面向对象编程

### 8.1 理解对象

#### 8.1.1 属性的类型

* 简单来说就是每个对象，可能有数据属性，数据属性有包括value在内的4个特性，可以用defineProperty()显式的修改。分别表示是否可以删除、循环、修改以及其本身的值。
* 也可能有访问器属性，多了get和set特性，是函数。

>对configurable已经设置为false的属性调用Object.defineProperty()就会出错。
>不能回滚的操作：definedProperty()和可计算属性都是不可回滚的，如果中途抛出错误，已创建的内容依旧会存在。

#### 8.1.4 合并对象

* Object.assign()方法接收一个目标对象和一个或多个原对象作为参数，然后将每个源对象中可枚举和自有属性复制到目标对象。将返回目标对象。
* 如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

#### 8.1.7 对象解构

* 可以在解构赋值的同时定义默认值哦
* 解构并不要求变量必须在解构表达式中声明，如果是给事先声明的变量赋值，则赋值表达式必须包含在一对括号中。

### 8.2 创建对象

#### 8.2.4 原型模式

* 使用原型对象的好处是，在它上面定义的属性和方法可以被对象实例共享

* 构造函数有一个prototype属性引用其原型对象，而这个原型对象也有一个constructor属性，引用这个构造函数。

* 实例通过__proto__链接到原型对象，它实际上指向隐藏特性[[Prototype]]

* 构造函数通过prototype属性链接到原型对象

* hasOwnProperty()方法会在属性存在于调用它的对象实例上时返回true

* in操作符会在可以通过对象访问指定属性时返回true，无论该属性是在实例上还是在原型上

* for-in和Object.keys()的枚举顺序是不确定的。Object.getOwnPropertyNames()、Object.getOwnPropertySymbols()和Object.assgin()的枚举顺序是确定的，先以升序枚举数值键，然后以插入顺序枚举字符串和符号键。

### 8.3 继承

#### 8.3.1 原型链

* 原型链的两个问题分别是：1、原型中包含的引用值会在所有实例间共享；2、子类型在实例化时不能给父类型的构造函数传参。

#### 8.3.2 盗用构造函数（对象伪装、经典继承）

* 在子类构造函数中调用父类构造函数

#### 8.3.3 组合继承（伪经典继承）

* 简而言之，组合继承是原型链和盗用构造函数实现继承一起用，既继承属性也继承方法。使用原型链继承原型上的属性和方法，通过盗用构造函数继承实例属性。保留了instanceof操作符和isPrototypeOf()方法识别合成对象的能力。

## 第10章 函数

### 10.9 函数内部

#### 10.9.1 arguments

* arguments还有一个arguments.callee属性，可以让函数逻辑与函数名解耦，不用担心以后函数名改变。

### 10.14 闭包

* 一个外部函数内部创建的函数（闭包）引用了外部函数的变量，这个闭包（内部函数）如果被返回并在其他地方被使用，依然引用着那个变量。因为对于两个函数的作用域链来说，外部函数后创建其作用域链，依次引用了外部函数的活动对象和全局变量对象。然后闭包（内部函数）因为引用了外部函数的活动对象，所以其作用域链会引用闭包（内部函数）的活动对象，外部对象的活动对象和全局变量对象。当外部函数执行完销毁后，其上下文作用域链会销毁，但其活动对象还被闭包作用域链引用，所以不会被销毁。

### 10.15 立即调用的函数表达式（IIFE）

* 简单来说就是ES6只后引入的块级作用域和let声明基本可以取代立即调用的函数表达式的写法了

### 10.16 私有变量

* 私有变量方法实现1，每个实例独有私有变量和私有函数，但是特权方法也是独有的

#### 10.16.1 静态私有变量

* 私有变量方法实现2，每个实例共享私有变量和私有函数和特权函数

#### 10.16.2 模块模式

* 在一个单例对象上实现。通过在匿名函数内定义私有变量和方法，返回一个字面量对象包含公共接口（类似于闭包的用法，可以向上访问到匿名函数内的私有变量）

#### 10.16.3 模块增强模式

* 增强的地方在可以返回特定类型实例，并添加特权/共有属性和方法

## 第十一章 期约与异步函数

### 11.2 期约

#### 11.2.2 期约基础

* Promise.reject()并没有照搬Promise.resolve()的幂等逻辑，如果给它传一个期约对象，则这个期约会成为它返回的拒绝期约的理由。

* 拒绝期约的错误并没有抛到执行同步代码的线程中，而是通过浏览器异步消息队列来处理的。因此，try/catch块并不能捕获该错误。唯一与之交互的方式就是使用期约的方式。

#### 11.2.3 期约的实例方法

* 如果只提供onRejected参数，那么就要在onResolved参数的位置传入undefined。

* 抛出异常会返回拒绝的期约

* 返回错误值不会触发上面的拒绝行为，而是会把错误对象包装在一个解决的期约中。

* onFinally处理程序没办法知道期约的状态是拒绝还是解决，主要用于清理代码。

* 如果给期约添加了多个处理程序，当期约状态变化时，相关处理程序会按照添加他们的顺序依次执行。

#### 11.2.4 期约连锁与期约合成

* all():如果所有期约都成功解决，则合成契约的解决值就是所有包含期约解决值的数组，按照迭代器顺序

* all()和race()合成的期约会静默处理所有包含期约的拒绝操作

### 11.3 异步函数

#### 11.3.1 异步函数

* 异步函数始终返回期约对象

* 拒绝期约的错误不会被异步函数捕获

* await关键字会暂停执行异步函数后面的代码，让出JavaScript运行时的执行线程

#### 11.3.3 异步函数策略

* 平行执行时，虽然期约不一定按照顺序执行，但await按顺序收到每个期约的值
