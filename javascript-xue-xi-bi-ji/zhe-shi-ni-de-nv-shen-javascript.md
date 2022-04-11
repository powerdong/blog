---
cover: https://s2.ax1x.com/2019/11/28/QPNJYQ.md.png
coverY: 0
---

# 这是你的女神！——JavaScript

> JavaScript ( JS ) 是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。

### Web 发展史

Mosaic 是互联网历史上第一个获普遍使用和能够显示图片的网页浏览器，于 1993 年问世

#### 浏览器组成

想要知道浏览器的工作原理以及浏览器的内核，我们要先知道浏览器的结构。

> 简单来说浏览器可以分为两部分，shell+内核。其中 shell 的种类相对比较多，内核则比较少。

Shell 是指浏览器的外壳：例如菜单，工具栏等。主要是提供给用户界面操 作，参数设置等等。它是调用内核来实现各种功能的。

内核才是浏览器的核心。内核是基于标记语言显示内容的程序或模块。内核又可分为 **渲染引擎**(语法规则和渲染)、**JS 引擎**、**其他模块**

* 2001 年发布的 IE6 首次实现了对 JS 引擎的优化和分离
* 2008 年 Google 发布最新浏览器 Chrome，引擎代号 V8，以速度快而闻名
* **JS 的组成**
  * ECMAScript——描述了该语言的语法和基本对象
  * 文档对象模型(DOM)——处理网页内容的方法和接口
  * 浏览器对象模型(BOM)——与浏览器进行交互的方法和接口
* **JS 的特色**
  * 解释性语言
  * 单线程
  * ECMA 标注
* **JS 执行队列**
  * 轮转时间片

### 变量声明

1. 变量名必须以`英文字母`、`_`、`$`开头
2. 变量名可以包括`英文字母`、`_`、`$`、`数字`
3. 不可以使用系统关键字、保留字作为变量名

```
// 声明、赋值分解
var a
a = 100
```

### JavaScript 规定了几种语言类型

![QPUcE8.png](https://s2.ax1x.com/2019/11/28/QPUcE8.png)

Javascript 有两种数据类型,分别是**基本数据类型**和**引用数据类型**

* 基本数据类型(原始值)
  * `Undefined`、`Null`、`Boolean`、`Number`、`String`、`Symbol` (ES6 新增,表示独一无二的值)
* 引用数据类型(引用值)
  * 统称为 Object 对象,主要包括对象、数组和函数。

#### 基础数据类型

1. 值是不可变的
2. 存放在栈区
3. 值的比较

#### 引用数据类型

1. 值是可变的
2. 同时保存在栈内存和堆内存
3. 比较是引用的比较

### 基本语法

1. 每行结束使用`;`
2. `=`、`+`、`-`两边都应该有空格

#### JS 错误分析

错误分成两种

1. 低级错误(语法解析错误)
2. 逻辑错误

js 语法错误会引发后续代码终止，但不会影响其他 JS 代码块

#### 运算操作符

* ```
  +
  ```
  * 数学运算、字符串连接
  * 任何数据类型加字符串都等于字符串
*   ```
    -
    ```

    、

    ```
    *
    ```

    、

    ```
    /
    ```

    、

    ```
    %
    ```

    、

    ```
    =
    ```

    、

    ```
    ()
    ```

    * `=`优先级最弱
    * `()`优先级最强
* `++`、`--`、`+=`、`-=`、`*=`、`/=`、`%=`

#### 比较运算符

* `>`、`<`、`==`、`>=`、`<=`、`!=`
* **字符串比较 ASCII**
* **NaN 不等于任何，包括 NaN**

#### 逻辑运算符

**会被转换为 false 的表达式有：**

* `null`
* `NaN`
* `0`
* 空字符串（`""` or `''` or \`\`）
* `undefined`

**逻辑运算符介绍**

* 逻辑与(`&&`)
  * `expr1 && expr2`
  * 若 expr1 可转换为 true，则返回 expr2；否则，返回 expr1。

```
a1 = true && true // t && t 返回 true
a2 = true && false // t && f 返回 false
a3 = false && true // f && t 返回 false
a4 = false && 3 == 4 // f && f 返回 false
a5 = 'Cat' && 'Dog' // t && t 返回 "Dog"
a6 = false && 'Cat' // f && t 返回 false
a7 = 'Cat' && false // t && f 返回 false
a8 = '' && false // f && f 返回 ""
a9 = false && '' // f && f 返回 false
```

* 逻辑或(`||`)
  * `expr1 || expr2`
  * 若 expr1 可转换为 true，则返回 expr1；否则，返回 expr2。

```
o1 = true || true // t || t 返回 true
o2 = false || true // f || t 返回 true
o3 = true || false // t || f 返回 true
o4 = false || 3 == 4 // f || f 返回 false
o5 = 'Cat' || 'Dog' // t || t 返回 "Cat"
o6 = false || 'Cat' // f || t 返回 "Cat"
o7 = 'Cat' || false // t || f 返回 "Cat"
o8 = '' || false // f || f 返回 false
o9 = false || '' // f || f 返回 ""
```

* 逻辑非(`!`)
  * `!expr`
  * 若 expr 可转换为 true，则返回 false；否则，返回 true。

```
n1 = !true // !t 返回 false
n2 = !false // !f 返回 true
n3 = !'' // !f 返回 true
n4 = !'Cat' // !t 返回 false
```

* 双重非(`!!`)运算符
  * 可能使用双重非运算符的一个场景，是显式地将任意值强制转换为其对应的布尔值

```
n1 = !!true // !!truthy 返回 true
n2 = !!{} // !!truthy 返回 true: 任何 对象都是 truthy 的…
n3 = !!new Boolean(false) // …甚至 .valueOf() 返回 false 的布尔值对象也是！
n4 = !!false // !!falsy 返回 false
n5 = !!'' // !!falsy 返回 false
n6 = !!Boolean(false) // !!falsy 返回 false
```

**短路计算**

由于逻辑表达式的运算顺序是从左到右，也可以用以下规则进行”短路”计算：

* `(some falsy expression) && (expr)` 短路计算的结果为假。
* `(some truthy expression) || (expr)` 短路计算的结果为真。

短路意味着上述表达式中的 expr 部分不会被执行，因此 expr 的任何副作用都不会生效

**运算优先级**

```
false && true || true // 结果为 true
false && (true || true) // 结果为 false
```

#### 条件语句

* `if`、`for`、`while`、`do while`、`switch case`
* `continue` 终止本次循环，进行下次循环
* `break` 终止循环
