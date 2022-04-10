# JS Plus 真香——初探 TypeScript

## Typescript初探

### TypeScript 简介

TypeScript 由 Microsoft 开发和维护的一种开源编程语言。他支持 JavaScript 的所有语法和语义，同时通过作为 ECMAScript 的超集来提供一些额外的功能，如类型检测和更丰富的语法。下图显示了 TypeScript 与 ES5，ES2015，ES2016 之间的关系。

![KOaMIs.png](https://s2.ax1x.com/2019/11/03/KOaMIs.png)

**TypeScript 是 JavaScript 的增强**

添加了可选择类型标注，增强了代码的可读性和可维护性，提供不断发展 JavaScript 的特性。

### 使用 TypeScript 的原因

JavaScript 是一门弱类型语言，变量的数据类型具有动态性，只有执行时才能确定变量的类型，这种后知后觉的认错方法会让开发者称为调试大师，但无益于编程能力的提升，还会降低开发效率。TypeScript 的类型机制可以有效杜绝由变量类型引起的误用问题，而且开发者可以控制对类型的监控程度，是严格限制变量类型还是宽松限制变量类型，都取决于开发者的开发需求。

总体而言，这些付出相对于代码的健壮性和可维护性，都是值得的。

### 环境配置

因为我们目前的主流浏览器还不支持直接运行 ts 文件，我们需要将其转换为 js 文件再进行运行。

#### 安装使用 TypeScript

获得 TypeScript 工具的主要方法有两种：

* 通过 npm（Node.js 程序包管理器）
* 通过安装 TypeScript 的 Visual Studio 插件

**通过 NPM 安装**

```
npm install -g typescript
```

**构建第一个 Ts 文件**

在编辑器中，新建一个 greeter.ts 文件输入以下 JavaScript 代码

```
// 在使用函数方法时，可以定义形参的类型
function myName(name: string) {
  console.log('my name is ', name)
}

myName('Lambda') // my name is Lambda
// 但是当你传入的值不是一个 String 类型时，VsCode 会报错
// myName(123) // ！！类型“123”的参数不能赋给类型“string”的参数

// 当在函数中只定义了一个形参时，当传入两个或多个实参也会报错
// myName('Lambda', '123') // 应有 1 个参数，但获得 2 个
```

**编译代码** 在命令行中，运行 TypeScript 编译器：

```
tsc greeter.ts
```

结果将是一个文件 greeter.js，其中包含与您输入的相同的 JavaScript。我们在 JavaScript 应用中使用 TypeScript 启动并运行！

### 定义接口

`TypeScript` 核心原则之一是对值所具有的结构进行类型检查，它是对行为的抽象，具体行动需要有类去实现，一般接口首字母大写。

```
interface Person {
  name: String
  age: Number
}

function person(person: Person) {
  console.log(`my name is ${person.name} age is ${person.age}`)
}

// 传入参数时使用对象传入
const people = {
  name: 'lambda',
  age: 20
}

person(people) // my name is lambda age is 20
```

### Class 类

在使用 class 类时，需要首先在开头声明参数及其类型

```
class Student {
  // 首先在开头声明参数及类型
  fullName: string // // 这个是对后文this.fullName类型的定义
  constructor(
    // 参数
    public firstName: string,
    public middleInitial: string,
    public lastName: string
  ) {
    this.fullName = firstName + ' ' + middleInitial + ' ' + lastName
  }
}

interface Person {
  firstName: string
  lastName: string
}

function greeter(person: Person) {
  console.log('Hello, ' + person.firstName + ' ' + person.lastName)
}

let user = new Student('Jane', 'M.', 'User')

greeter(user) // Hello, Jane User
```

### 类型

#### String 类型

一个保存字符串的文本，类型声明为 string。**类型声明可大写也可小写**

```
let name: String = 'lambda'
let name2: string = 'lambda2'
```

#### Number 类型

```
let number: number = 10
```

#### Boolean 类型

boolean 是 true 或 false 的值

`let isBool3: boolean = new Boolean(1)` 就会编译报错 因为 `new Boolean(1)` 生成的是一个 Bool 对象。

```
let isDone: boolean = false / true
```

#### Array 类型

数组是 Array 类型。然而数组是一个集合，我们还需要指定在数组中的元素的类型。

我们通过`Array<type>`或`type[]`语法为数组中元素指定类型

```
let arr: number[] = [1, 2, 5, 8, 3, 9]
let arr: Array<string> = ['5', '4', '2', '7', '9']
```

#### Any 类型

any 是默认的类型，其类型的变量允许任何类型的值：

```
let notSure: any = 10
notSure = 'hello'
let notSure2: any[] = [1, '2', false]
```

#### Void 类型

JavaScript 没有空值 Void 的概念，在 TypeScript 中，可以用 void 表示没有任何返回值的函数：

```
function alertName(): void {
  console.log('My name is lambda')
}

alertName()
```

#### 元组类型

长度固定，类型固定

```
const x: [string, number] = ['hello', 1]
```

#### 枚举类型

列出所有可用的值，一个枚举的默认初始值是 0。

```
enum Color {
  red = 1,
  green,
  blue
}

const c: Color = Color.blue

const colorName: string = Color[2]

console.log(c, colorName) // 3 green
```

#### undefined 和 null

```
const u: undefined = undefined
const n: null = null
```

#### 联合类型

表示取值可以是多种类型中的一种

```
let myFavoriteNumber: string | number // 连接符 |
myFavoriteNumber = 'seven'
myFavoriteNumber = 7
```

#### 类型断言

相信我，我知道自己在干什么

```
let someValue: any = 'this is a string'
// 强制将 someValue 转换为string
let strLength: number = (<string>someValue).length //“尖括号”语法
// 强制将 someValue 转换为 string
let strLength1: number = (someValue as string).length //一个为as语法
```

> 在`let`和`const`在社么时候使用的问题上，当你的变量要改变值的时候使用`let`，否则都可以使用`const`

### 函数

#### 定义函数类型

我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 TypeScript 能够根据返回语句自动推断出返回值类型，因此我们通常省略它

```
function add(x: number, y: number): number {
  return x + y
} //有名函数
let myAdd = function(x: number, y: number): number {
  return x + y
} //匿名函数
```

#### 可选参数和默认参数

JavaScript 里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是 undefined 。 在 TypeScript 里我们可以在参数名旁使用`?`实现可选参数的功能。

使用`b?:number` 定义 b 为可选参数

```
function add(a: number, b?: number): void {
  console.log(a + b)
}

add(1, 4) // 5
add(4) // NaN b 未传，值为 undefined
// add(1, 8, 3) // 应有 1-2 个参数，但获得 3 个。
```

如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined 值来获得默认值。

### 接口

TypeScript 的核心原则之一是对值所具有的结构进行类型检查。在 TypeScript 里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

```
// Label 用来描述接口内的结构
interface Label {
  label: String
}
```

传入的值对象内必须包含该属性，并且类型需要匹配

**创建一个正方形**

```
interface Square {
  color: string
  area: number
}

interface SquareConfig {
  color?: string
  width?: number
}

function createSquare(config: SquareConfig): Square {
  let newSquare = { color: 'white', area: 100 }
  if (config.color) {
    newSquare.color = config.color
  }
  if (config.width) {
    newSquare.area = config.width * config.width
  }
  return newSquare
}

const mySquare = createSquare({ color: 'red' })
console.log(mySquare) // { color: 'red', area: 100 }
```

#### 只读属性

一些对象属性只能在对象刚刚创建的时候修改它的值

`TypeScript` 具有 `ReadonlyArray<T>` 类型，它与 `Array<T>` 相似只是把所有的可变方法去掉了，确保数组创建后再也不能被修改

> readonly Vs const 如果我们要把他当做一个变量就使用 const ,若为属性则使用 readonly

```
interface Point {
  readonly x: number
  readonly y: number
}

let p: Point = { x: 10, y: 20 } // 初始化后将无法修改
// p.x = 20 // 会报错！！
```

#### 函数类型

接口能够描述 `JavaScript` 中对象拥有的各种各样的外形，描述了带有的普通对象之外，接口也可以描述成函数类型;`他有一个调用签名，参数列表和返回值类型的函数定义，参数列表里的每一个参数都需要名字和类型，函数的参数名不需要与接口里定义的名字相匹配，如果你没有指定参数类型，TypeScript` 的类型系统会推断出参数类型

```
interface SearchFunc {
  (source: string, subString: string): boolean
}
let MySearch: SearchFunc
MySearch = function(source: string, subString: string) {
  let result = source.search(subString)
  return result > -1
}
```

#### 可索引类型

与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如 `a[10]` 或 `ageMap["daniel"]`。 可索引类型具有一个索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。

```
interface StringArray {
  [index: number]: string
}

let MyArray: StringArray
MyArray = ['lambda1', 'lambda2', 'lambda3', 'lambda4']
console.log(MyArray[2]) // lambda3
```

### 类

#### 继承接口

和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里

```
interface Shape {
  color: string
}

interface PenStroke {
  penWidth: number
}
// 继承多个接口，使用 `,` 分割
interface Square extends Shape, PenStroke {
  sideLength: number
}

let s = <Square>{}
s.color = 'blue'
s.penWidth = 100
s.sideLength = 10
```

#### 存储器

TypeScript 支持通过 getters/setters 来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。

对于存取器有下面几点需要注意的：

* 首先，存取器要求你将编译器设置为输出 ECMAScript 5 或更高。 不支持降级到 ECMAScript 3。
* 其次，只带有 `get` 不带有 `set` 的存取器自动被推断为 `readonly`。 这在从代码生成 `.d.ts` 文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值

```
class Hello {
  private _name: string
  private _age: number
  get name(): string {
    return this._name
  }
  set name(value: string) {
    this._name = value
  }
  get age(): number {
    return this._age
  }
  set age(age: number) {
    if (age > 0 && age < 100) {
      console.log('年龄在0-100之间') // 年龄在0-100之间
      return
    }
    this._age = age
  }
}

let hello = new Hello()
hello.name = 'lambda' // 调用 set 方法
hello.age = 23 // 调用 set 方法
console.log(hello.name) // // 调用 get 方法 ==> lambda
```

#### 静态属性

当属性只存在于类本身上面而不是类实例上，叫做静态成员标识符 static

当在类中定义了静态属性时，使用`类名.`的方式使用

```
class Grid {
  static origin = { x: 0, y: 0 }
}
// 要通过 Grid.origin.x 的方式访问
```

#### 抽象类

作为其他派生类的基类使用，他们一般不会直接被实例化，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。抽象方法的语法和接口方法相似，都只是定义方法签名，但不包括方法体。抽象类可包含成员的实现细节，必须包含 `abstract` 关键字标识和访问修饰符

```
// 定义抽象类
abstract class Animal {
  // 定义抽象方法
  abstract makeSound(): void
  move(): void {
    console.log('roaming the earch...')
  }
}
```

### 泛型

软件工程中，我们不仅要创建一致的定义良好的 API ，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能

一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

如下代码，我们给 `identity` 函数添加了类型变量 `T` ，`T` 帮助我们捕获用户传入的类型（比如：`string`）。我们把这个版本的 `identity` 函数叫做泛型，因为它可以适用于多个类型。代码中 `output` 和 `output2` 是效果是相同的，第二种方法更加普遍，利用了类型推断 —— 即编译器会根据传入的参数自动地帮助我们确定`T`的类型

```
function identity<T>(arg: T):T {
  return arg
}

let output = identity<string>('myIsString') // 明确写出来
let output2 = identity(arg: 'myIsString') // 进行类型推断，判断类型
```

#### 泛型接口

```
interface GenericIdentityFn {
  <T>(arg: T): T
}
function identity<T>(arg: T): T {
  return arg
}
let myIdentity: GenericIdentityFn = identity
```

#### 泛型类

泛型类看上去和泛型接口差不多，泛型类使用(<>)括起泛型类型，跟在类名后面

```
class GenericNumber<T> {
  zeroValue:T,
  add:(x:T,y:T)=>T
}
let myGenericNumber = new GeneriNumber<number>()
```

类有两个部分：静态部分和实例部分，泛型类指的实例部分，所以静态属性不能使用这个泛型类型，定义接口来描述约束条件

#### 泛型约束

```
interface Lengthwise {
  length: number
}
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length)
  return arg
}
```

`extends` 继承了一个接口进而对泛型的数据结构进行了限制

#### 交叉类型

```
function extend<T, U>(first: T, second: U): T & U {
  let result = {} as T & U
  // code ...
  return result
}
```

#### 联合类型

```
let padding: string | number // 两种类型之一
```

联合类型在使用方法时只能使用某一个方法，交叉类型在使用方法时可以使用多个对象方法
