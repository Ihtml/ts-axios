##TypeScript简介

[TypeScript](http://www.typescriptlang.org/) 是微软开源的 JavaScript 的一个超集，它提供了**类型系统**，弥补了JavaScript这门动态弱类型语言的不足。它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。

使用TypeScript很大的**增强了js代码的可读性和可维护性**。它在编译阶段就能发现语法错误，配合编辑器的智能提醒，在开发阶段就能杜绝大部分的语法错误。

相比于JavaScript, TypeScript 更可控、更容易重构、更适合大型项目、更容易维护。

TypeScirpt可以通过npm安装

`npm install -g typescript`

安装后，可以通过`tsc`命令编译.ts后缀的TypeScript文件。

## TypeScript常用语法

### 类型

TypeScript 支持与 JavaScript 几乎相同的数据类型。包括：布尔值、数字、字符串、数组、null和undefined等

```typescript
let flag:boolean = true
let num: number = 123
let str: string = 'abc'
let list: number[] = [1, 2, 3]
let strList: Array<string> = ['a', 'b', 'c']
let u: undefined = undefined
let n: null = null
let obj: object = {id: 1}
```

默认情况下，null和undefined可以赋值给其他声明了类型的变量，但如果编译的时候加上`--strictNullChecks`就会报错。

TypeScript还可以定义ECMAScript 标准提供的内置对象为类型，比如：`Boolean`、`Error`、`Date`、`RegExp` 等；以及DOM 和 BOM 提供的内置对象，如：`Document`、`HTMLElement`、`Event`、`NodeList` 等。

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;

let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

如果一个变量没有声明类型，那它第一次赋值后，TypeScript会默认给它加上该值对应的类型，如果给这个变量赋值其他类型的值时，会报错。

```typescript
let a = 123
a = 'abc' // 会报错
```



此外，typescript还提供了其他一些类型：

**元组**

元组类型允许表示一个**已知元素数量和类型的数组**，各元素的类型不必相同。 比如，可以定义一对值分别为 `string` 和 `number` 类型的元组。

```typescript
let x: [string, number]
x = ['hello', 10] // OK
x = [10, 'hello'] // Error
```

访问一个已知索引的元素，会得到正确的类型。

```typescript
console.log(x[0].substr(1)) // OK
console.log(x[1].substr(1)) // Error, 'number' 不存在 'substr' 方法
```

可以只给元组中一项单独赋值

```typescript
x[0] = 'test'
```

但是当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。

```typescript
let x: [string, number]
x = ['abc', 123] // right
x = ['abc'] // wrong
```

**枚举**

使用枚举类型可以为一组数值赋予名字，适合用于取值被限定在一定范围内的场景，比如星期：

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
console.log(Days["Sun"]); // 0
console.log(Days["Mon"]); // 1
console.log(Days["Tue"]); // 2
console.log(Days["Sat"]); // 6

console.log(Days[0]); // "Sun"
console.log(Days[1]); // "Mon"
console.log(Days[2]); // "Tue"
console.log(Days[6]); // "Sat"
```

枚举成员默认会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射。

可以给枚举项手动赋值, 未手动赋值的枚举项会接着上一个枚举项递增（+1）。

```typescript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查

```typescript
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

**any**

`any` 类型用来表示一个变量可以是任意类型。

比如一个数组可能有多个类型的元素，那可以用any来定义它。

```typescript
let list: any[] = [123, 'abc', true, {}]
```

**void**

`void` 类型表示没有任何类型。

比如一个函数没有返回值时：

```typescript
function sayHello(): void {
  console.log('Hello!')
}
```

声明 `void` 类型的变量，只能被赋予 `undefined` 和 `null`

**never**

`never` 类型表示的是那些**永不存在的值的类型**。一般用于错误处理函数。

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message)
}

// 推断的返回值类型为never
function fail(): never {
  return error("Something failed")
}
```

**类型断言**

类型断言用来手动指定一个值的类型。

有两种语法：<类型>值，值 as 类型

```typescript
	let someValue: any = 'abcd'
  let valength = (<string>someValue).length
  let vlength = (someValue as string).length
```

注意一点：在React中使用TypeScript时，类型断言中只能使用as语法。

### 接口

接口用于**对象的形状进行描述**，一般首字母大写。

```typescript
interface Obj1 {
    key1: string;
    key2: number;
    key3?: object;
    readonly key4: number[];
    [propName: string]: any;
}
function fn1(obj: Obj1) {
    console.log(obj);
}
let obj: Obj1 = {
    key1: 'abc',
    key2: 123,
    key3: {},
    key4: [1, 2, 3]
}
fn1(obj)
```

**赋值的时候，变量的形状必须和接口的形状保持一致**。定义的变量比接口少属性和多属性都是不允许的。

如果不确定是不是需要某个属性，可以使用**可选属性**，接口的属性名字定义后面加一个`?`表示。可选属性的含义是该属性可以不存在。

如果希望对象中的一些字段在创建后不能再被赋值，可以定义它为**只读属性**，通过属性名字前加`readonly`定义。

如果一个对象可能具有某些做为特殊用途使用的额外属性，那可以使用**字符串索引签名**。

```typescript
interface Obj {
	[propName: string]: any
}
```

**函数类型**

JavaScript中函数也是对象，接口可以描述对象拥有的各种外形，同样接口也可以描述函数类型, **函数的参数名不需要与接口里定义的名字相匹配**。

```typescript
interface FuncType {
    (arg1: string, arg2:number): boolean
}
let funcType: FuncType

funcType = function (a1:string, a2: number): boolean {
    return false
}
// 可简写，TS会自动检查函数参数类型和返回值类型，如果跟接口定义不一致会报错
funcType = function (a1, a2) {
    return false
}
```

**索引类型**

可索引类型具有一个 索引签名，它**描述了对象索引的类型**，还有**相应的索引返回值类型。** 

```typescript
// 当用 number 去索引 StringArray 时会得到 string 类型的返回值。
interface StringArray {
  [index: number]: string
}
let myArray: StringArray
myArray = ['Bob', 'Fred']
let myStr: string = myArray[0]
```

TypeScript 支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是**数字索引的返回值必须是字符串索引返回值类型的子类型**。 这是因为当使用 `number` 来索引时，JavaScript 会将它转换成`string` 然后再去索引对象。

可以将索引签名设置为只读，就防止了给索引赋值：

**类类型**

可以在接口中描述一个属性或方法，在类里实现它。因为接口只描述了类的公共部分，所有不会检查类的私有成员。

```typescript
interface ClockInterface {
  currentTime: Date
  setTime(d: Date)
}

class Clock implements ClockInterface {
  currentTime: Date
  setTime(d: Date) {
    this.currentTime = d
  }
  constructor(h: number, m: number) { }
}
```

类有两种类型：**实例部分类型**和**静态部分类型（构造器类型**）。不能用一个类去实现构造器类型的接口。

```typescript
interface ClockConstructor {
  new (hour: number, minute: number): ClockInterface
}
interface ClockInterface {
  tick()
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
  return new ctor(hour, minute)
}

class DigitalClock implements ClockInterface {
  constructor(h: number, m: number) { }
  tick() {
    console.log('beep beep')
  }
}
class AnalogClock implements ClockInterface {
  constructor(h: number, m: number) { }
  tick() {
    console.log('tick tock')
  }
}

let digital = createClock(DigitalClock, 12, 17)
let analog = createClock(AnalogClock, 7, 32)
```

和类一样，接口也可以相互**继承**。可以方便从一个接口里复制成员到另一个接口。

```typescript
interface Shape {
  color: string
}

interface PenStroke {
  penWidth: number
}
interface Square extends Shape, PenStroke {
  sideLength: number
}
let square = {} as Square
square.color = 'blue'
square.sideLength = 10
square.penWidth = 5.0
```

**混合类型**

JavaScript是动态语言，有时候会希望一个对象同时具有多种类型，比如可以同时做为函数和对象使用，并带有额外属性。

```typescript
interface Counter {
    (start: number): string
    interval: number
    reset(): void
}

function getCounter(): Counter {
    let counter = (function (start: number) { }) as Counter
    counter.interval = 123
    counter.reset = function () { }
    return counter
}

let c = getCounter()
c(10)
c.reset()
c.interval = 5.0
```

**接口继承类**

当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 **接口同样会继承到类的 `private` 和 `protected` 成员**。 这意味着当你**创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。**

当你有一个庞大的继承结构时这很有用，但要指出的是你的代码只在子类拥有特定属性时起作用。 这个子类除了继承至基类外与基类没有任何关系。例：

```typescript
class Control {
  private state: any
}
interface SelectableControl extends Control {
  select(): void
}
class Button extends Control implements SelectableControl {
  select() { }
}
class TextBox extends Control {
  select() { }
}
// Error：“ImageC”类型缺少“state”属性。
class ImageC implements SelectableControl {
  select() { }
}
```

在上面的例子里，`SelectableControl` 包含了 `Control` 的所有成员，包括私有成员 `state`。 因为 `state` 是私有成员，所以只能够是 `Control` 的子类们才能实现 `SelectableControl` 接口。 因为只有 `Control` 的子类才能够拥有一个声明于`Control` 的私有成员 `state`，这对私有成员的兼容性是必需的。

在 `Control` 类内部，是允许通过 `SelectableControl` 的实例来访问私有成员 `state` 的。 实际上，`SelectableControl` 接口和拥有 `select` 方法的 `Control` 类是一样的。`Button`和 `TextBox` 类是 `SelectableControl` 的子类（因为它们都继承自`Control` 并有 `select` 方法），但 `ImageC` 类并不是这样的。

### 类

JS生成实例对象的传统方法是通过构造函数, ES6 中引入了Class（类）， [ECMAScript 6 入门 - Class](http://es6.ruanyifeng.com/#docs/class)

**public private 和 protected修饰符**

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 `public`、`private` 和 `protected`。

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的