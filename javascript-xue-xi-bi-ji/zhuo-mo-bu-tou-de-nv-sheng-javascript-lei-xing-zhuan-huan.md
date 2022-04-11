# 捉摸不透的女生——JavaScript类型转换

### 起因

凡事都有个来源和起因，这个题不是我在哪篇文章看到的，也不是我瞎乱造出来的，是在看一个重学 Js 类型转换视频中老师使用`console.log({} + [])` 在控制台输出了`[object Object]`，不过看了人家的视频自己也需要动手敲一下嘛，于是我没有在编辑器里边编辑，偷了个懒直接在控制台里边编辑打印。巧了 竟然返回结果不一样！！

&#x20;![ZN3qi9.png](https://s2.ax1x.com/2019/07/04/ZN3qi9.png)

对结果出乎意料，本着实事求是的精神，探寻事物的本质，不断努力追根溯源，总算明白了最后的结果，最后的收获总算把 js 的隐式类型转换刨根问底的搞清楚了，也更加深入的明白了为什么 Js 是弱类型语言了。

### 开胃小菜

```
Number({1:2})
[] + {}
[] + function() {}
2 + [2,3]
10 + function() {}
null + undefined
[] - 5
'6' * []
null % true
true/[7]
"A" - "A" + 2
```

[答案](zhuo-mo-bu-tou-de-nv-sheng-javascript-lei-xing-zhuan-huan.md#da-an)

### Js 数据类型(6 种)

Javascript 有两种数据类型,分别是`基本数据类型`和`引用数据类型`。其中基本数据类型包括 `Undefined`、`Null`、`Boolean`、`Number`、`String`、`Symbol` (ES6 新增,表示独一无二的值),而引用数据类型统称为 Object 对象,主要包括对象、数组和函数。

**这里抛开 Symbol 不论述**

在 js 中可以把任意一种数据类型转换为_数字_，_字符串\*、\*布尔值_。

在运行`true + 12`时 Js 并不会报错，由于 Js 是一种弱类型语言，Js 会自动转换，将其输出。 看到这是不是觉得 Js 编译很贴心(暖暖的),但是呢，有利就有弊，有的时候也就是因为它会自动转换类型，会出现你意想不到的结果。

#### 数据类型转换分为显式和隐式

#### **`Number()`显式**

```
Number(undefined); //NaN
Number(null); //0

Number(true); //1
Number(false); //0

Number(""); //0
Number(" "); //0
Number("12"); //12
Number("012"); //12

Number("0xff90"); //65424
Number("5e5"); //500000
Number("k"); //NaN

Number({}); //NaN
Number([]); //0
Number(function() {}); //NaN
Number([""]); //0
Number([2]); //2
Number(["2"]); //2
Number([2, 3]); //NaN
```

**总结**

* ```
  Number()
  ```
  1. **undefined**–>>`NaN`
  2. **null**————>>`0`
  3. **布尔值**——–true 为 `1`，false 为 `0`
  4. 字符串
     1. 空字符串、空格字符串转为 `0`
     2. 非空字符串，并且内容为纯数字(包含进制与科学计数法)转对应的数字
     3. 其余都是`NaN`
  5. **数字** 原来的数字
  6. 对象
     1. 对象 函数转 `NaN`
     2. 空数组转为 `0`，数组里只有一个数据并且这个数据能转为数字，则转为对应的数字，其他都转成`NaN`

#### **`String()`显式**

```
String(null); //null
String([1, 2]); //1,2
String({}); //[object Object]
String(function() {}); //function(){}
```

**总结**

* ```
  String()
  ```
  1. `基本数据类型`，`null`，`undefined` 的结果就是给数据加上引号变成字符串
  2. 对象
     1. 数组的结果为把所有的中括号去掉，然后在外面加个引号
     2. 对象的结果为”`[object Object]`“(除了日期对象)
     3. 函数的结果为在函数整体外面加个引号

#### **`Boolean()`显式**

```
Boolean(null); //false
Boolean(undefined); //false
Boolean(); //false
Boolean(+0); //false
Boolean(""); //false
Boolean(" "); //true
Boolean({}); //true
Boolean({ a: 1 }); //true
Boolean([]); //true
```

**总结**

* Boolean()
  1. **undefined**——>>false
  2. **null**———–>>false
  3. 数字
     * `+0`、`-0`、`NaN`转布尔值的结果为`false`，其他的转布尔值结果为`true`
  4. **布尔值** 转为对应的值
  5. 字符串
     * 空字符串转布尔值为`false`，其他(包括空格字符串)的都转为`true`
  6. **对象**转布尔值都是`true`

#### 答案

```
Number({1:2})//NaN
[] + {}  //"[object Object]"
[] + function() {}   //"function() {}"
2 + [2,3]   //"22,3"
10 + function() {} //"10function() {}"
null + undefined  //NaN
[] - 5   //-5
'6' * [] //0
null % true //0
true/[7] //1/7 = 0.14285714285714285
"A" - "A" + 2  //NaN
```

#### 原理

> 对象的原型上都有一个 toString()和 valueOf()方法

* **toString()**
  * 返回对象的字符串表现形式
* **valueOf()**
  * 返回对象对应的数值
* **数字、字符串、布尔值**
  * `valueOf`—-数据本身(原始值形式)
  * `toString`—-数据转换为字符串的形式
* **数组**
  * `valueOf`—-数据本身(对象形式)
  * `toString`—-去掉中括号，外面加个引号(本质为调用 `join(",")`后的结果)
* **函数**
  * `valueOf`—-数据本身(对象式)
  * `toString`—-在数据外面加个引号
* **对象**
  * `valueOf`—-数据本身(对象形式)
  * `toString`—-“`[object Object]`“

```
//Number()
var obj = {};

obj.valueOf = function() {
  alert("你调用了valueOf()方法");
  // 神奇的发现它会调用valueOf方法
  return 88;
};

obj.toString = function() {
  alert("你调用了toString()方法");
  // 神奇的发现它会调用toString方法
};

console.log(Number(obj));
```

**`Number` 参数为对象的转换原理**

1. 调用对象的`valueOf`方法，如果返回原始类型的值，再使用`Number`函数，不在进行后续步骤
2. 如果`valueOf`方法返回的还是对象，则调用`toString`方法
3. 如果`toString`方法返回原始类型的值，则对该值使用`Number`方法，不进行后续步骤
4. 如果`toString`方法后返回的还是对象，就报错(一般不会出现)

```
//String()
var obj = {};

obj.toString = function() {
  alert("你调用了toString()方法");
  // 神奇的发现它会调用toString方法
  return 88;
};

obj.valueOf = function() {
  alert("你调用了valueOf()方法");
  // 神奇的发现它会调用valueOf方法
};

console.log(String(obj));
```

**`String` 参数为对象的转换原理**

1. 调用对象的`toString`方法，如果返回原始类型的值，再使用`String`函数，不在进行后续步骤
2. 如果`toString`方法返回的还是对象，则调用`valueOf`方法
3. 如果`valueOf`方法返回原始类型的值，则对该值使用`String`方法，不进行后续步骤
4. 如果`valueOf`方法后返回的还是对象，就报错(一般不会出现)

### 隐式转换

#### 出现的场景

> 1. 不同类型的数据间运算，比较
> 2. 对费布尔值类型的数据求布尔值
> 3. 条件语句的括号里

#### 隐式类型转数字

**出现的场景**

> `+` `-` `*` `/` `%` 加号`+`运算符里不能出现字符串或对象类型 `+` `-`(正负) 某些比较运算符

[去看 Number 显式转换](zhuo-mo-bu-tou-de-nv-sheng-javascript-lei-xing-zhuan-huan.md#number-xian-shi)

```
// 转数字
1 + undefined   //NaN
1 + NaN  //1
null + undefined //NaN
true + false // 1
3 + '6'  //'36'
// 旁边如果有对象  调用String方法
2 + []   //'2'
2 + {}   //'2[object Object]'

// 其余的 - / % 都会转数字
[] - 5 //-5
```

#### 隐式类型转字符串

[去看 String 显式转换](zhuo-mo-bu-tou-de-nv-sheng-javascript-lei-xing-zhuan-huan.md#string-xian-shi)

**出现场景**

> 1. 有字符串的加法运算
> 2. 有对象的加法运算
> 3. 某些比较运算符
> 4. 调用 alert、document.write 方法

```
console.log(1 + "2"); //12
console.log("1" + undefined); //1undefined
console.log("1" + []); //1
console.log("1" + {}); //1[object Object]

console.log(2 + [2, 3]); //22,3
console.log(2 + function() {}); //2function(){}

console.log([] + {}); //[object Object]
console.log({} + null); //[object Object]null
console.log([] + undefined); //undefined

alert({ a: 12 }); //[object Object]
alert(function() {}); //function(){}

document.write({}); //[object Object]
```

来看看下面这道题输出的结果是什么吧(注意过程)

```
console.log(++[[]][+[]] + [+[]]); //10
// ++[[]][+[]]    +     [+[]];
// ++[[]][0]      +     [0];
// ++[]           +     [0];
// ++0            +     [0];
// 1              +     [0];
// 10
```

#### 隐式类型转布尔值

[去看 Boolean 显式转换](zhuo-mo-bu-tou-de-nv-sheng-javascript-lei-xing-zhuan-huan.md#boolean-xian-shi)

**出现场景**

> 1. 取反运算
> 2. 1 个叹号表示，把这个数据转换成布尔值后取它的反值
> 3. 2 个叹号表示，把这个数值转成布尔值
> 4. 三目运算符
> 5. 条件语句的小括号里
> 6. 逻辑运算符

```
var a = [0];
if ([0]) {
  // 对象转布尔值 为 true
  console.log(1);
} else {
  console.log(2);
}
```

#### 大小比较的原理

1. 如果比较两边有字符串，那会对比他们各自对应的 Unicode 码值
2. 如果两边都为原始类型数据，都把所有数据转成数字进行比较，**NaN 与任何数据对比都是 false**
3. 有对象数据的比较，先调用 valueOf 再调用 toString 方法，然后看另一数据的类型，如果另一数据为字符串，则转为字符串对比，如果另一数据为数字，则转为数字对比。

```
console.log("A" > "a"); //false 65>97
console.log("大" > "小"); //false 22823>23567

console.log(true > false); //true
console.log("k" > 5); //false NaN > 5
console.log(undefined > false); //false NaN>0
console.log(null >= 0); //true 0>=0
console.log("大" > "小"); //false 22823>23567

console.log([] > 0); //false 0>0
console.log({} > 1); //false NaN>1
console.log({} > true); //false NaN>1
console.log({} > "/"); //true 91>47
console.log({} > "["); //true "[object Object]">"["  第一个相同比较第二个

console.log({ a: 1 } < function() {}); //false "[object Object]"<"function(){}"
```

**总结**

1. 字符串与字符串，字符串与对象，对象与对象进行比较时，都会转成字符串，然后对比 Unicode 码值
2. 其他类型数值，转成数字对比

#### 相等比较原理

1. 不同类型的原始类型数据，把所有的数据转成数字后进行对比
2. null 和 undefined 除了它们自己与自己，自己与对方相等，与其他任何数据都不相等
3. 对象与原始类型比较时，把对象转换成原始值，在进行比较
4. 对象与对象类型比较时，比较的是他们的引用地址，除非引用地址相同，否则都不相等

```
//转数字
console.log(12 == "12"); //true
console.log(0 == ""); //true
console.log(1 == true); //true
console.log(2 == false); //false
console.log("true" == true); //false
console.log(" " == false); //true
console.log(undefined == 0); //false
console.log(undefined == ""); //false
console.log(undefined == false); //false
console.log(undefined == undefined); //true
console.log(null == 0); //false
console.log(null == ""); //false
console.log(null == false); //false
console.log(null == undefined); //true
console.log(null == null); //true
console.log([] == 1); //true
console.log(0 == []); //true
console.log([1] == true); //true
console.log([1, "a", 3] == "1,a,3"); //true
console.log([] == undefined); //false
console.log({ a: 1 } == 1); //false
console.log(null == [null]); //false
console.log([] == []); //false
console.log({} == {}); //false

// 这里需要抛出一个有意思的现象，读者可以自行验证
console.log(null <= 0); //true
console.log(null >= 0); //true
```

**总结**

* 原始类型对比，转成数字对比
* 对象类型对比，把对象转成字符串，然后看对方的类型
  * 如果不一样，都转成数字对比
  * 对象与对象比较，比较引用地址
*   ```
    null
    ```

    和

    ```
    undefined
    ```

    * 自己与自己，自己与对方相等
    * 与其他任何数据都不相等

#### 逻辑运算符

> ```
> &&` 和 `||
> ```

**&&：** 两边的数据必须要都成立，整个表达式才能成立

**||：** 有一方成立，整个表达式成立

```
console.log("powerdong" && 2 + 1); //3
console.log(![] && 2 + 1); //false
let n1 = 1;
n1 - 1 && (n1 += 5);
console.log(n1); //1

function fn() {
  console.log("我会执行吗");
}

n == 1 && fn();
//与下方表达式意思相同
/*if(n==1){
*   fn()
}*/
console.log(true && "power" && 2 - 2 && "abc" && true); //0
console.log("powerdong" || 2 - 1); //powerdong

let n1 = 1;
n1 - 1 || (n1 += 5);
console.log(n1); //6

// 容错
function fn(text) {
  text = text || "powerdong";
}

fn();
```

**总结**

*   && (找

    ```
    false
    ```

    )

    * 左边为真的话，直到找到为 `false` 结果的值返回
    * 左边不为真，直接返回左边表达式的值
*   || (找

    ```
    true
    ```

    )

    * 左边不为真的话，直到找到为 `true` 结果的值返回
    * 左边为真，直接返回左边表达式的值

#### 逗号运算符

> 小逗号，大用处

```
// 参数分隔
console.log(1, 2, 3); //1,2,3
// 表达式
console.log((1, 2, 3)); //3
// alert只能接收一个参数
alert(1, 2, 3); //1
alert((1, 2, 3)); //3
var a = 10;
bar b = (a++,20,30);
console.log(a,b);//11 30
for (var i = 0, j = 0, k; i < 6, j < 10; i++, j++) {
  //var i = 0, j = 0, k;--->参数分隔符
  //i < 6, j < 10;--->从左到右都会执行，但是只能返回一个结果 j<10
  //i++, j++ ---> 从左到右都会执行
  k = i + j;
}
console.log(k); //18
```

**总结**

1. 逗号运算符可以**从左往右依次执行表达式**
2. 将**最后**的那个表达式结果返回
3. 优先级最低
4. `alert` 只能接收一个参数

面试题分享，请将下面的 a 返回 20，b 返回 10

```
var a = 10;
var b = 20;
```

我们在这里可以想到很多方法，比如增加一个中间值，相加相减，ES6 的解构赋值等。这里，我们可以使用逗号`','`运算符巧妙解决这个问题。

> `a=[b][b=a,0]` 这样就可以完美的解决了此问题

```
var a = 10;
var b = 20;

a = [b][b = a, 0)];
// a = [20][b=10,0]
// a = [20][0]
// a = 20  b = 10
```

我们继续

```
var a = 10, //逗号分隔
  b = 20;
function fn() {
  // return  只能返回一个值  10
  return a++, b++, 10;
}
// 此时fn执行，但是在最后才执行
console.log(a, b, fn()); //10 20 10
// 此时 a == 11, b == 21
var c = fn(); //10
// 此时 a == 12, b == 22
console.log(a, b, c); //12 22 10
```
