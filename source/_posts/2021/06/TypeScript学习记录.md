---
title: TypeScript学习记录
date: 2021-06-22 18:00:00
tags:
  - 前端
  - TypeScript
categories:
  - 前端
  - TypeScript
---

按照惯例，先上官方文档

[TypeScript 中文手册](https://typescript.bootcss.com)

<!--more-->

# 基础类型

## 数据类型

TypeScript 支持与 JavaScript 几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用。

- 布尔值 `boolean`
- 浮点数 `number`
  和 JavaScript 一样，TypeScript 里的所有数字都是浮点数
- 字符串 `string`
  双引号 `""` 或单引号 `''` 表示，
  还可使用模板字符串，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号 `` ` `` 包围，并且以 `${ expr }` 这种形式嵌入表达式
- 数组 `[]` 或数组泛型 `Array<元素类型>`
- 元组 `Tuple`
  元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
- 枚举 `enum`
  枚举类型提供的一个便利是你可以由枚举的值得到它的名字。
- 任意值 `any`
  有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。
  Object 类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法。
- 空值 `void`
  它表示没有任何类型
- `null` 和 `undefined`
  `null` 和 `undefined` 两者各自有自己的类型分别叫做 `null` 和 `undefined` 。
  默认情况下 `null` 和 `undefined` 是所有类型的子类型。
- `never`
  表示的是那些永不存在的值的类型。
  是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是 `never` 的子类型或可以赋值给 `never` 类型（除了 `never` 本身之外）

## 类型断言

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

- “尖括号”语法 `<变量类型>变量值`
- `as` 语法 `变量值 as 变量类型`

两种形式是等价的，但当在 TypeScript 里使用 JSX 时，只有 as 语法断言是被允许的。

# 变量声明

let 和 const 是 JavaScript 里相对较新的变量声明方式。 像我们之前提到过的，let 在很多方面与 var 是相似的，但是可以帮助大家避免在 JavaScript 里常见一些问题。 const 是对 let 的一个增强，它能阻止对一个变量再次赋值。

## `var` 声明

`var` 声明有奇怪的作用域规则，可以在包含它的函数，模块，命名空间或全局作用域内部任何位置被访问，包含它的代码块对此没有什么影响。有些人称此为 var 作用域或函数作用域。函数参数也使用函数作用域。

文档中的例子：

```typescript
function f(shouldInitialize: boolean) {
  if (shouldInitialize) {
    var x = 10
  }

  return x
}

f(true) // returns '10'
f(false) // returns 'undefined'
```

可能引发的错误：

- 多次声明同一个变量并不会报错

  ```typescript
  // 里层的for循环会覆盖变量i，因为所有i都引用相同的函数作用域内的变量
  function sumMatrix(matrix: number[][]) {
    var sum = 0
    for (var i = 0; i < matrix.length; i++) {
      var currentRow = matrix[i]
      for (var i = 0; i < currentRow.length; i++) {
        sum += currentRow[i]
      }
    }
    return sum
  }
  ```

- 变量获取怪异

  ```typescript
  for (var i = 0; i < 10; i++) {
    setTimeout(function () {
      console.log(i)
    }, 100 * i)
  }
  // 期望值： 0 1 2 3 4 5 6 7 8 9
  // 实际值： 10 10 10 10 10 10 10 10 10 10
  ```

  `setTimeout` 在若干毫秒后执行一个函数，并且是在 `for` 循环结束后。 `for` 循环结束后，`i` 的值为 `10`。 所以当函数被调用的时候，它会打印出 `10`！

  一个通常的解决方法是使用立即执行的函数表达式（IIFE）来捕获每次迭代时 i 的值：

  ```typescript
  for (var i = 0; i < 10; i++) {
    // capture the current state of 'i'
    // by invoking a function with its current value
    ;(function (i) {
      setTimeout(function () {
        console.log(i)
      }, 100 * i)
    })(i)
  }
  // 参数i会覆盖for循环里的i，但是因为我们起了同样的名字，所以我们不用怎么改for循环体里的代码。
  // 输出值： 0 1 2 3 4 5 6 7 8 9
  ```

## `let` 声明

当用 `let` 声明一个变量，它使用的是词法作用域或块作用域。

- 不同于使用 `var` 声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或 `for` 循环之外是不能访问的。

- 拥有块级作用域的变量的另一个特点是，它们不能在被声明之前读或写。虽然这些变量始终“存在”于它们的作用域里，但在直到声明它的代码之前的区域都属于暂时性死区。

- 但我们仍然可以在一个拥有块作用域变量被声明前获取它，只是我们不能在变量声明前去调用那个函数。

使用 `var` 声明同名变量时，不管声明多少次，得到的只有 1 个对象。但 `let` 在一个作用域里的同名变量只能声明一次。

## `const` 声明

拥有与 `let` 声明相同的作用域规则，但给变量声明赋值后不能再重新赋值。

`const` 声明的变量所引用的值是不可变的，但变量的内部状态是可以改变的。

TypeScript 允许将对象的成员设置成只读。

## 解构

- 解构数组

  ```typescript
  // 相当于使用索引
  let input = [1, 2]
  let [first, second] = input
  console.log(first) // outputs 1
  console.log(second) // outputs 2

  // 使用...语法创建剩余变量
  let [first, ...rest] = [1, 2, 3, 4]
  console.log(first) // outputs 1
  console.log(rest) // outputs [ 2, 3, 4 ]

  // swap variables
  ;[first, second] = [second, first]
  ```

- 解构对象

  ```typescript
  let o = {
    a: 'foo',
    b: 12,
    c: 'bar',
  }
  let { a, b } = o

  // 可以用没有声明的赋值
  ;({ a, b } = { a: 'baz', b: 101 })

  // 使用...语法创建剩余变量
  let { a, ...passthrough } = o
  let total = passthrough.b + passthrough.c.length
  ```

- 属性重命名
- 默认值

  ```typescript
  function keepWholeObject(wholeObject: { a: string; b?: number }) {
    let { a, b = 1001 } = wholeObject
  }
  ```

- 函数声明

  ```typescript
  // 指定默认值
  // 首先，需要在默认值之前设置其格式。
  // 其次，需要在解构属性上给予一个默认或可选的属性用来替换主初始化列表。
  function f({ a, b = 0 } = { a: '' }): void {
    // ...
  }
  f({ a: 'yes' }) // ok, default b = 0
  f() // ok, default to {a: ""}, which then defaults b = 0
  f({}) // error, 'a' is required if you supply an argument

  // 解构表达式要尽量保持小而简单
  ```

## 展开

展开操作符正与解构相反。它允许你将一个数组展开为另一个数组，或将一个对象展开为另一个对象。

```typescript
// 解构数组
let first = [1, 2]
let second = [3, 4]
let bothPlus = [0, ...first, ...second, 5] // bothPlus = [0, 1, 2, 3, 4, 5]

// 解构对象
let defaults = { food: 'spicy', price: '$$', ambiance: 'noisy' }
let search = { ...defaults, food: 'rich' } // search = { food: "rich", price: "$$", ambiance: "noisy" }
```

**注意**

- 对象的展开比数组的展开要复杂的多。像数组展开一样，它是从左至右进行处理，但结果仍为对象。这就意味着出现在展开对象后面的属性会覆盖前面的属性。
- 对象展开仅包含对象自身的可枚举属性。展开一个对象实例时，会丢失其方法。
- TypeScript 编译器不允许展开泛型函数上的类型参数。

# 接口 - interface

TypeScript 的核心原则之一是对值所具有的结构进行类型检查。它有时被称做“鸭式辨型法”或“结构性子类型化”。

在 TypeScript 里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

在调用函数时，传入的对象参数实际上会包含很多属性，但是编译器只会检查那些必需的属性是否存在，并且其类型是否匹配。

## 可选属性

带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个 `?` 符号。

可选属性的好处之一是可以对可能存在的属性进行预定义，好处之二是可以捕获引用了不存在的属性时的错误。

## 只读属性

可以在属性名前用 readonly 来指定只读属性。满足对象创建后不能修改属性值的限制。

TypeScript 具有 `ReadonlyArray<T>` 类型，它与 `Array<T>` 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。

```typescript
let a: number[] = [1, 2, 3, 4]
let ro: ReadonlyArray<number> = a
ro[0] = 12 // error!
ro.push(5) // error!
ro.length = 100 // error!
a = ro // error!

// 不过可以用类型断言重写
a = ro as number[]
```

最简单判断该用 readonly 还是 const 的方法是看要把它做为变量使用还是做为一个属性。

做为变量使用的话用 const，若做为属性则使用 readonly。

## 额外的属性检查

对象字面量会被特殊对待而且会经过额外属性检查，当将它们赋值给变量或作为参数传递的时候。如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。

```typescript
interface SquareConfig {
  color?: string
  width?: number
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  // ...
}

// error: 'colour' not expected in type 'SquareConfig'
let mySquare = createSquare({ colour: 'red', width: 100 })
```

绕开这些检查非常简单。最简便的方法是使用类型断言。但最佳的方式是能够添加一个字符串索引签名。最后一种跳过这些检查的方式是将这个对象赋值给一个另一个变量。

```typescript
// as断言
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig)

// 如果SquareConfig带有上面定义的类型的color和width属性，并且还会带有任意数量的其它属性，那么我们可以这样定义它
interface SquareConfig {
  color?: string
  width?: number
  [propName: string]: any
}

// 因为squareOptions不会经过额外属性检查，所以编译器不会报错
let squareOptions = { colour: 'red', width: 100 }
let mySquare = createSquare(squareOptions)
```

## 函数类型

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。TypeScript 编译器会要求对应位置上的参数类型是兼容的，如果不指定类型，TypeScript 的类型系统会推断出参数类型。

## 可索引的类型

可索引类型具有一个索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。

支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。

```typescript
class Animal {
  name: string
}
class Dog extends Animal {
  breed: string
}

// 错误：使用'string'索引，有时会得到Animal!
interface NotOkay {
  [x: number]: Animal
  [x: string]: Dog
}
```

字符串索引签名能够很好的描述 `dictionary` 模式，并且它们也会确保所有属性与其返回值类型相匹配。

可以将索引签名设置为只读，这样就防止了给索引赋值。

## 类类型

### `implements` 实现接口

与 C# 或 Java 里接口的基本作用一样，TypeScript 也能够用它来明确的强制一个类去符合某种契约。

也可以在接口中描述一个方法，在类里实现它。接口描述了类的公共部分，而不是公共和私有两部分。它不会帮你检查类是否具有某些私有成员。

### 类静态部分与实例部分的区别

类是具有两个类型的：静态部分的类型和实例的类型。

当一个类实现了一个接口时，只对其实例部分进行类型检查。 constructor 存在于类的静态部分，所以不在检查的范围内。因此，我们应该直接操作类的静态部分。

### `extends` 继承接口

和类一样，接口也可以相互继承。这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里，并通过继承多个接口创建合成接口。

## 混合类型

> 不懂

一个例子就是，一个对象可以同时做为函数和对象使用，并带有额外的属性。

```typescript
interface Counter {
  (start: number): string
  interval: number
  reset(): void
}

function getCounter(): Counter {
  let counter = <Counter>function (start: number) {}
  counter.interval = 123
  counter.reset = function () {}
  return counter
}

let c = getCounter()
c(10)
c.reset()
c.interval = 5.0
```

## 接口继承类

当接口继承了一个类类型时，它会继承类的成员但不包括其实现。

接口同样会继承到类的 private 和 protected 成员。 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。

# 类

## 继承

类从基类中继承了属性和方法。派生类通常被称作子类，基类通常被称作超类。

派生类在自己的构造函数里必须调用 super()，即基类的构造函数。且访问 this 的属性之前，一定要调用 super()。

## 公共，私有与受保护的修饰符

### `public`

在 TypeScript 里，成员都默认为 `public`

### `private`

不能在声明它的类的外部访问 `private` 成员。

### `protected`

`protected` 成员在派生类中仍然可以访问。

### `readonly` 修饰符

可以使用 `readonly` 关键字将属性设置为只读的。只读属性必须在声明时或构造函数里被初始化。

## 存取器

TypeScript 支持通过 getters/setters 来截取对对象成员的访问。它能帮助你有效的控制对对象成员的访问。

- 存取器要求你将编译器设置为输出 ECMAScript 5 或更高。不支持降级到 ECMAScript 3
- 只带有 `get` 不带有 `set` 的存取器自动被推断为 `readonly` 。

## 静态属性

用 `static` 定义，这些属性存在于类本身上面而不是类的实例上。

## 抽象类

不同于接口，抽象类可以包含成员的实现细节。 `abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。

抽象方法必须包含 abstract 关键字并且可以包含访问修饰符。

# 函数 - function

> 在 JavaScript 里，函数可以使用函数体外部的变量。当函数这么做时，我们说它‘捕获’了这些变量。
> 函数中使用的捕获变量不会体现在类型里。 实际上，这些变量是函数的隐藏状态并不是组成 API 的一部分。

## 函数类型

TypeScript 能够根据返回语句自动推断出返回值类型。

函数类型包含两部分：参数类型和返回值类型。当写出完整函数类型的时候，这两部分都是需要的。函数的类型只是由参数类型和返回值组成的。

- 我们以参数列表的形式写出参数类型，为每个参数指定一个名字和类型。只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。
- 对于返回值，我们在函数和返回值类型之前使用(=>)符号，如果函数没有返回任何值，也必须指定返回值类型为 void 而不能留空。

## 函数参数

### 可选参数和默认参数

TypeScript 里的每个函数参数都是必须的。 这是指编译器检查用户是否为每个参数都传入了值，传递给一个函数的参数个数必须与函数期望的参数个数一致。

- JavaScript 里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是 undefined。
- 在 TypeScript 里我们可以在参数名旁使用?实现可选参数的功能。可选参数必须跟在必须参数后面。

在 TypeScript 里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的值是 undefined 时。 它们叫做有默认初始化值的参数。

与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined 值来获得默认值。

### 剩余参数

在 JavaScript 里，你可以使用 arguments 来访问所有传入的参数。

在 TypeScript 里，你可以把所有参数收集到一个变量里，剩余参数会被当做个数不限的可选参数。可以一个都没有，同样也可以有任意个。编译器创建参数数组，名字是你在省略号（...）后面给定的名字，你可以在函数体内使用这个数组。

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + ' ' + restOfName.join(' ')
}

let employeeName = buildName('Joseph', 'Samuel', 'Lucas', 'MacKinzie')

let buildNameFun: (fname: string, ...rest: string[]) => string = buildName
```

## `this`

JavaScript 里，`this` 的值在函数被调用的时候才会指定。

当回调函数被调用时，它会被当成一个普通函数调用，`this` 将为 `undefined`，可以通过 `this` 参数来避免错误。

## 重载

为同一个函数提供多个函数类型定义来进行函数重载。

为了让编译器能够选择正确的检查类型，它与 JavaScript 里的处理流程相似。 它查找重载列表，尝试使用第一个重载定义。 如果匹配的话就使用这个。 因此，在定义重载的时候，一定要把最精确的定义放在最前面。

# 泛型 - generic

在像 C#和 Java 这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。这样用户就可以以自己的数据类型来使用组件。

无法创建泛型枚举和泛型命名空间。

example：

```typescript
function identity<T>(arg: T): T {
  return arg
}
// 定义了泛型函数后，可以用两种方法使用
// 第一种是，传入所有的参数，包含类型参数
let output = identity<string>('myString') // type of output will be 'string
// 第二种，利用了类型推论 – 即编译器会根据传入的参数自动地帮助我们确定T的类型
let output = identity('myString') // type of output will be 'string'
```

泛型接口

```typescript
interface GenericIdentityFn<T> {
  (arg: T): T
}

function identity<T>(arg: T): T {
  return arg
}

let myIdentity: GenericIdentityFn<number> = identity
```

泛型类

泛型类使用（<>）括起泛型类型，跟在类名后面。

```typescript
class GenericNumber<T> {
  zeroValue: T
  add: (x: T, y: T) => T
}

let myGenericNumber = new GenericNumber<number>()
myGenericNumber.zeroValue = 0
myGenericNumber.add = function (x, y) {
  return x + y
}
```

泛型约束

```typescript
interface Lengthwise {
  length: number
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length) // Now we know it has a .length property, so no more error
  return arg
}

loggingIdentity(3) // Error, number doesn't have a .length property

loggingIdentity({ length: 10, value: 3 }) // OK
```

在泛型约束中使用类型参数

可以声明一个类型参数，且它被另一个类型参数所约束。

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key]
}

let x = { a: 1, b: 2, c: 3, d: 4 }

getProperty(x, 'a') // okay
getProperty(x, 'm') // error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.
```

在泛型里使用类类型

在 TypeScript 使用泛型创建工厂函数时，需要引用构造函数的类类型。

```typescript
class BeeKeeper {
  hasMask: boolean
}

class ZooKeeper {
  nametag: string
}

class Animal {
  numLegs: number
}

class Bee extends Animal {
  keeper: BeeKeeper
}

class Lion extends Animal {
  keeper: ZooKeeper
}

function createInstance<A extends Animal>(c: new () => A): A {
  return new c()
}

createInstance(Lion).keeper.nametag // typechecks!
createInstance(Bee).keeper.hasMask // typechecks!
```

# 枚举 - enum

使用枚举我们可以定义一些有名字的数字常量。枚举通过 `enum` 关键字来定义。

枚举是在运行时真正存在的一个对象。其中一个原因是因为这样可以从枚举值到枚举名进行反向映射。生成的代码中，枚举类型被编译成一个对象，它包含双向映射（name -> value）和（value -> name）。 引用枚举成员总会生成一次属性访问并且永远不会内联。

当访问枚举值时，为了避免生成多余的代码和间接引用，可以使用常数枚举，在 `enum` 关键字前使用 `const` 修饰符。常数枚举不可能有计算成员。

外部枚举用来描述已经存在的枚举类型的形状。外部枚举和非外部枚举之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常数成员。对于非常数的外部枚举而言，没有初始化方法时被当做需要经过计算的。

# 类型推论 - type inference

当需要从几个表达式中推断类型时候，会使用这些表达式的类型来推断出一个最合适的通用类型。如果没有找到最佳通用类型的话，类型推断的结果为联合数组类型。

# 参考文章

- [优雅的在 vue 中使用 TypeScript](https://zhuanlan.zhihu.com/p/99343202)
