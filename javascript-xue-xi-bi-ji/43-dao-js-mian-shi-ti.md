# 43道JS面试题

**1. 下面代码输出是什么?**

```
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

* A: `Lydia` 和 `undefined`
* B: `Lydia` 和 `ReferenceError`
* C: `ReferenceError` 和 `21`
* D: `undefined` 和 `ReferenceError`答案 \`\`\`\`\`\`\`\`\`\`\`\` \`\`\`\`\`\`\`\`\`\`

关于 `let`、`var`和`function`:

* `let`的’创建’过程被提升了，但是初始化没有提升。
* `var`的’创建’和’初始化’都被提升了
* `function`的’创建’、’初始化’和’赋值’都被提升了。

**2. 下面代码的输出是什么?**

```
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

* A: `0 1 2` and `0 1 2`
* B: `0 1 2` and `3 3 3`
* C: `3 3 3` and `0 1 2`

<details>

<summary>答案</summary>

\


</details>

**3. 下面代码的输出是什么?**

```
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

shape.diameter();
shape.perimeter();
```

* A: `20` and `62.83185307179586`
* B: `20` and `NaN`
* C: `20` and `63`
* D: `NaN` and `63`

<details>

<summary>答案</summary>

\
\


</details>

**4. 下面代码的输出是什么?**

```
+true;
!"Lydia";
```

* A: `1` and `false`
* B: `false` and `NaN`
* C: `false` and `false`

<details>

<summary>答案</summary>

\
\


</details>

**5. 哪个选项是不正确的?**

```
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

* A: `mouse.bird.size`
* B: `mouse[bird.size]`
* C: `mouse[bird["size"]]`
* D: All of them are valid

<details>

<summary>答案</summary>

\
\
\


</details>

**6. 下面代码的输出是什么?**

```
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

* A: `Hello`
* B: `undefined`
* C: `ReferenceError`
* D: `TypeError`

<details>

<summary>答案</summary>

\
\
\


</details>

**7. 下面代码的输出是什么?**

```
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

<details>

<summary>答案</summary>

\
\
\


</details>

**8. 下面代码的输出是什么?**

```
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
freddie.colorChange("orange");
```

* A: `orange`
* B: `purple`
* C: `green`
* D: `TypeError`

<details>

<summary>答案</summary>

\


</details>

**9. 下面代码的输出是什么?**

```
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

* A: `{}`
* B: `ReferenceError: greetign is not defined`
* C: `undefined`

<details>

<summary>答案</summary>

\


</details>

**10. 当我们这样做时会发生什么?**

```
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

* A: `Nothing, this is totally fine!`
* B: `SyntaxError. You cannot add properties to a function this way.`
* C: `undefined`
* D: `ReferenceError`

<details>

<summary>答案</summary>

\


</details>

**11. 下面代码的输出是什么?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = () => this.firstName + this.lastName;

console.log(member.getFullName());
```

* A: `TypeError`
* B: `SyntaxError`
* C: `Lydia Hallie`
* D: `undefined undefined`

<details>

<summary>答案</summary>

\


</details>

**12. 下面代码的输出是什么?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```

* A: `Person {firstName: "Lydia", lastName: "Hallie"} and undefined`
* B: `Person {firstName: "Lydia", lastName: "Hallie"} and Person {firstName: "Sarah", lastName: "Smith"}`
* C: `Person {firstName: "Lydia", lastName: "Hallie"} and {}`
* D: `Person {firstName: "Lydia", lastName: "Hallie"} and ReferenceError`

<details>

<summary>答案</summary>

\


</details>

**13. 所有对象都有原型.**

* A: 对
* B: 错误

<details>

<summary>答案</summary>

\


</details>

**14. 下面代码的输出是什么?**

```
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

* A: `NaN`
* B: `TypeError`
* C: `"12"`
* D: `3`

<details>

<summary>答案</summary>

\


</details>

**15. 下面代码的输出是什么?**

```
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

* A: `1 1 2`
* B: `1 2 2`
* C: `0 2 2`
* D: `0 1 2`

<details>

<summary>答案</summary>

\
\


</details>

**16. 下面代码的输出是什么?**

```
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = "Lydia";
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

* A: `Lydia 21 ["", "is", "years old"]`
* B: `["", "is", "years old"] Lydia 21`
* C: `Lydia ["", "is", "years old"] 21`

<details>

<summary>答案</summary>

\


</details>

**17. 下面代码的输出是什么?**

```
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

* A: `You are an adult!`
* B: `You are still an adult.`
* C: `Hmm.. You don't have an age I guess`

<details>

<summary>答案</summary>

\
\


</details>

**18. 下面代码的输出是什么?**

```
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

* A: `"number"`
* B: `"array"`
* C: `"object"`
* D: `"NaN"`

<details>

<summary>答案</summary>

\


</details>

**19. 事件传播的三个阶段是什么？?**

* A: 目标 > 捕获 > 冒泡
* B: 冒泡 > 目标 > 捕获
* C: 目标 > 冒泡 > 捕获
* D: 捕获 > 目标 > 冒泡

<details>

<summary>答案</summary>

\


</details>

**20. 下面代码的输出是什么?**

```
function getAge() {
  "use strict";
  age = 21;
  console.log(age);
}

getAge();
```

* A: `21`
* B: `undefined`
* C: `ReferenceError`
* D: `TypeError`

<details>

<summary>答案</summary>

\


</details>

**21. 下面代码的输出是什么?**

```
const sum = eval("10*10+5");
```

* A: `105`
* B: `"105"`
* C: `TypeError`
* D: `"10\*10+5"`

<details>

<summary>答案</summary>

\


</details>

**22. cool\_secret 可以访问多长时间?**

```
sessionStorage.setItem("cool_secret", 123);
```

A：永远，数据不会丢失。 B：用户关闭选项卡时。 C：当用户关闭整个浏览器时，不仅是选项卡。 D：用户关闭计算机时。

<details>

<summary>答案</summary>

\


</details>

**23. 下面代码的输出是什么?**

```
var num = 8;
var num = 10;

console.log(num);
```

* A: `8`
* B: `10`
* C: `SyntaxError`
* D: `ReferenceError`

<details>

<summary>答案</summary>

\


</details>

**24. 下面代码的输出是什么?**

```
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

* A: `false true false true`
* B: `false true true true`
* C: `true true false true`
* D: `true true true true`

<details>

<summary>答案</summary>

\


</details>

**25. 下面代码的输出是什么?**

```
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

* A:`{ a: "one", b: "two" }`
* B: `{ b: "two", a: "three" }`
* C: `{ a: "three", b: "two" }`
* D: `SyntaxError`

<details>

<summary>答案</summary>

\


</details>

**26. JavaScript 全局执行上下文为你创建了两个东西:全局对象和 this 关键字.**

* A: 对
* B: 错误
* C: 视情况而定

<details>

<summary>答案</summary>

\


</details>

**27. 下面代码的输出是什么?**

```
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

* A: `1 2`
* B: `1 2 3`
* C: `1 2 4`
* D: `1 3 4`

<details>

<summary>答案</summary>

\


</details>

**28. 下面代码的输出是什么?**

```
String.prototype.giveLydiaPizza = () => {
  return "Just give Lydia pizza already!";
};

const name = "Lydia";

name.giveLydiaPizza();
```

* A: `"Just give Lydia pizza already!"`
* B: `TypeError: not a function`
* C: `SyntaxError`
* D: `undefined`

<details>

<summary>答案</summary>

\
\


</details>

**29. 下面代码的输出是什么?**

```
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

* A: `123`
* B: `456`
* C: `undefined`
* D: `ReferenceError`

<details>

<summary>答案</summary>

\


</details>

**30. 下面代码的输出是什么?**

```
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
```

* A: `First` `Second` `Third`
* B: `First` `Third` `Second`
* C: `Second` `First` `Third`
* D: `Second` `Third` `First`

<details>

<summary>答案</summary>

\
\
\
\
\
\


</details>

**31. 单击按钮时 event.target 是什么?**

```
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

* A: `div 外部`
* B: `div 内部`
* C: `button`
* D: `所有嵌套元素的数组.`

<details>

<summary>答案</summary>

\


</details>

**32. 单击下面的 html 片段打印的内容是什么?**

```
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

* A: `p` `div`
* B: `div` `p`
* C: `p`
* D: `div`

<details>

<summary>答案</summary>

\


</details>

**33. 下面代码的输出是什么?**

```
const person = { name: "Lydia" };

function sayHi(age) {
  console.log(`${this.name} is ${age}`);
}

sayHi.call(person, 21);
sayHi.bind(person, 21);
```

* A: `undefined is 21 Lydia is 21`
* B: `function function`
* C: `Lydia is 21 Lydia is 21`
* D: `Lydia is 21 function`

<details>

<summary>答案</summary>

\


</details>

**34. 下面代码的输出是什么?**

```
function sayHi() {
  return (() => 0)();
}

typeof sayHi();
```

* A: `"object"`
* B: `"number"`
* C: `"function"`
* D: `"undefined"`

<details>

<summary>答案</summary>

\


</details>

**35. 下面这些值哪些是假值?**

```
0;
new Number(0);
("");
(" ");
new Boolean(false);
undefined;
```

* A: `0, '', undefined`
* B: `0, new Number(0), '', new Boolean(false), undefined`
* C: `0, '', new Boolean(false), undefined`
* D: `所有都是假值`

<details>

<summary>答案</summary>

\


\
\
\
\


</details>

**36. 下面代码的输出是什么?**

```
console.log(typeof typeof 1);
```

* A: `"number"`
* B: `"string"`
* C: `"object"`
* D: `"undefined"`

<details>

<summary>答案</summary>

\


</details>

**37. 下面代码的输出是什么?**

```
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

* A: `[1, 2, 3, 7 x null, 11]`
* B: `[1, 2, 3, 11]`
* C: `[1, 2, 3, 7 x empty, 11]`
* D: `SyntaxError`

<details>

<summary>答案</summary>

\


</details>

**38. 下面代码的输出是什么?**

```
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

* A: `1` `undefined` `2`
* B: `undefined` `undefined` `undefined`
* C: `1` `1` `2`
* D: `1` `undefined` `undefined`

<details>

<summary>答案</summary>

\


</details>

**39. JavaScript 中的所有内容都是…**

* A：原始或对象
* B：函数或对象
* C：技巧问题！只有对象
* D：数字或对象

<details>

<summary>答案</summary>

\


</details>

**40. 下面代码的输出是什么?**

```
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

* A: `[0, 1, 2, 3, 1, 2]`
* B: `[6, 1, 2]`
* C: `[1, 2, 0, 1, 2, 3]`
* D: `[1, 2, 6]`

<details>

<summary>答案</summary>

\


</details>

**41. 下面代码的输出是什么?**

```
!!null;
!!"";
!!1;
```

* A: `false` `true` `false`
* B: `false` `false` `true`
* C: `false` `true` `true`
* D: `true` `true` `false`

<details>

<summary>答案</summary>

\


</details>

**42. setInterval 方法的返回值什么?**

```
setInterval(() => console.log("Hi"), 1000);
```

* A：一个唯一的 id
* B：指定的毫秒数
* C：传递的函数
* D：undefined

<details>

<summary>答案</summary>

\


</details>

**43. 下面代码的返回值是什么?**

```
[..."Lydia"];
```

* A: `["L", "y", "d", "i", "a"]`
* B: `["Lydia"]`
* C: `[[], "Lydia"]`
* D: `[["L", "y", "d", "i", "a"]]`

<details>

<summary>答案</summary>

答案: A\
字符串是可迭代的。 扩展运算符将迭代的每个字符映射到一个元素

</details>
