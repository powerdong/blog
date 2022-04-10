# 收集的JS使用技巧

## &#x20;JavaScript常用方法

数组是 JS 最常见的一种数据结构，咱们在开发中也经常用到,在这篇文章中,提供一些小技巧,帮助咱们提高开发效率。

### 删除数组中的重复项

```javascript
const fruits = [
  'banana',
  'apple',
  'orange',
  'watermelon',
  'banana',
  'apple',
  'orange'
]

// 第一种方式
const uniqueFruits = Array.from(new Set(fruits))
console.log(uniqueFruits) // [ 'banana', 'apple', 'orange', 'watermelon' ]

// 第二种方式
const uniqueFruits2 = [...new Set(fruits)]
console.log(uniqueFruits2) // [ 'banana', 'apple', 'orange', 'watermelon' ]
```

### 替换数组中的特定值

```javascript
const userNames = ['Lambda1', 'Lambda2', 'Lambda3', 'Lambda4']
userNames.splice(0, 2, 'Lambda替换1', 'Lambda替换2')
console.log(userNames) // [ 'Lambda替换1', 'Lambda替换2', 'Lambda3', 'Lambda4' ]
```

### Array.form 达到 .map 的效果

```javascript
const friends = [
  {
    name: 'Lambda',
    age: 18
  },
  {
    name: 'Lambda1',
    age: 19
  },
  {
    name: 'Lambda2',
    age: 20
  },
  {
    name: 'Lambda3',
    age: 21
  },
  {
    name: 'Lambda4',
    age: 22
  },
  {
    name: 'Lambda5',
    age: 23
  }
]

const friendsNames = Array.from(friends, ({ name }) => name)
console.log(friendsNames) // [ 'Lambda', 'Lambda1', 'Lambda2', 'Lambda3', 'Lambda4', 'Lambda5' ]
```

### 置空数组

```javascript
const friends = [
  {
    name: 'Lambda',
    age: 18
  },
  {
    name: 'Lambda1',
    age: 19
  },
  {
    name: 'Lambda2',
    age: 20
  },
  {
    name: 'Lambda3',
    age: 21
  },
  {
    name: 'Lambda4',
    age: 22
  },
  {
    name: 'Lambda5',
    age: 23
  }
]

friends.length = 0
console.log(friends) // []
// 为什么要用 array.length = 0 这种写法来清空 array，用 array = [] 不是更好吗？因为数组的 length 属性一般是不去修改的。
// array = []的话会产生一个新的空数组并让array指向它。这样做有一个问题：其他引用array的对象不会受影响，得到的还是之前的值，而array.length = 0会直接改变原数组。
```

### 将数组转换为对象

```javascript
const users = ['Lambda1', 'Lambda2', 'Lambda3', 'Lambda4']
const usersObj = {
  ...users
}

console.log(usersObj) // { '0': 'Lambda1', '1': 'Lambda2', '2': 'Lambda3', '3': 'Lambda4' }
```

### 将数据填充数组

```javascript
const newArray = new Array(10).fill('1')
console.log(newArray) // ['1', '1', '1', '1','1', '1', '1', '1','1', '1']
```

### 数组合并

```javascript
const fruits = ['banana', 'apple', 'orange', 'watermelon']
const meat = ['poultry', 'beef', 'fish']
const vegetables = ['potato', 'tomato', 'cucumber']
const food = [...fruits, ...meat, ...vegetables]
console.log(food) // ['banana','apple','orange','watermelon','poultry', 'beef','fish','potato','tomato',  'cucumber']
```

### 求两个数组的交集

```javascript
const numOne = [0, 1, 2, 3, 4, 5]
const numTwo = [4, 5, 6, 7, 8, 9]
const duplicatedValues = [...new Set(numOne)].filter((item) =>
  numTwo.includes(item)
)
console.log(duplicatedValues) // [4,5]
```

### 从数组中删除虚值

> 在 JS 中，虚值有 false, 0，’’， null, NaN, undefined。咱们可以 .filter() 方法来过滤这些虚值。

```javascript
const mixedArr = [0, 'blue', '', NaN, 9, true, undefined, 'white', false]
const trueArr = mixedArr.filter(Boolean)

console.log(trueArr) // [ 'blue', 9, true, 'white' ]
```

### 从数组中获取随机值

```javascript
const names = ['Lambda1', 'Lambda2', 'Lambda3', 'Lambda4']
const randomNames = names[Math.floor(Math.random() * names.length)]

console.log(randomNames) // Lambda3
```

### 反转数组

```javascript
const names = ['Lambda1', 'Lambda2', 'Lambda3', 'Lambda4']
const reverseNames = names.reverse()

console.log(reverseNames) // [ 'Lambda4', 'Lambda3', 'Lambda2', 'Lambda1' ]
```

### lastIndexOf() 方法

```javascript
const nums = [1, 5, 8, 1, 84, 184, 81, 3]
const lastIndex = nums.lastIndexOf(5)

console.log(lastIndex) // 1
```

### 对数组中的所有值求和

```javascript
const nums = [1, 5, 2, 8, 9]
const sum = nums.reduce((x, y) => x + y)
console.log(sum) // 25
```

### 邮箱验证

```javascript
var pattern = /^([A-Za-z0-9_\-\.\u4e00-\u9fa5])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,8})\$/
```
